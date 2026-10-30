# 03 - Container Storage Interface (CSI) Architecture

## 1. What Is the Container Storage Interface (CSI)?

The **Container Storage Interface (CSI)** is an industry-standard, specification-agnostic gRPC interface that enables third-party storage vendors (such as AWS, Google Cloud, Microsoft Azure, Dell EMC, NetApp, Ceph, Portworx, and Pure Storage) to write storage plugins for container orchestrators without touching Kubernetes core source code.

Before CSI was established in Kubernetes 1.13+, all storage plugins were **in-tree**. This meant the Go code required to create AWS EBS volumes or mount Azure Disks lived directly inside the `kubernetes/kubernetes` GitHub repository. In-tree plugins suffered from massive architectural disadvantages:
1. **Severe Security and Release coupling:** A critical bug or security flaw in a vendor storage driver required compiling, testing, and releasing an entirely new version of the entire Kubernetes binary (`kube-controller-manager`).
2. **Privilege Overreach:** Storage vendor code ran with root cluster-admin privileges inside the core Kubernetes control plane binaries.
3. **Vendor Bottlenecks:** Storage vendors could not ship new disk features, hotfixes, or performance tuning independently; they had to wait for Kubernetes' 4-month upstream release cycle.

CSI solved these problems permanently by converting storage plugins into **out-of-tree** microservices that interact with Kubernetes through standardized gRPC sockets.

```text
+---------------------------------------------------------------------------------------------------+
|                        IN-TREE (LEGACY) VS OUT-OF-TREE CSI (MODERN)                               |
+---------------------------------------------------------------------------------------------------+

  LEGACY ARCHITECTURE (IN-TREE):
  ┌──────────────────────────────────────────────────────────────────────────────────┐
  │ kube-controller-manager (Monolithic Binary)                                      │
  │   ├── AWS EBS driver Go code (baked inside k8s binary)                           │
  │   ├── Azure Disk driver Go code (baked inside k8s binary)                        │
  │   └── GCE PD driver Go code (baked inside k8s binary)                            │
  └──────────────────────────────────────────────────────────────────────────────────┘
  Problem: Fixing a driver bug required upgrading the entire Kubernetes cluster version!

  MODERN ARCHITECTURE (OUT-OF-TREE CSI):
  ┌───────────────────────────────┐               gRPC Unix Socket
  │ Kubernetes Core API & Kubelet │◄────────────────────────────────────────────────┐
  └───────────────────────────────┘                                                 │
                                                                                    ▼
                                                  ┌───────────────────────────────────────────────┐
                                                  │ Vendor CSI Driver (Independent Microservice)  │
                                                  │ Developed, versioned, & deployed via Helm/YAML│
                                                  └───────────────────────────────────────────────┘
```

---

## 2. CSI Architecture: The Controller and Node Plugin Separation

A production CSI driver is deployed as two distinct workloads in the cluster: a centralized **Controller Deployment** (high availability with leader election) and a distributed **Node DaemonSet** (running on every worker machine).

```text
+---------------------------------------------------------------------------------------------------+
|                         CSI DEPLOYMENT TOPOLOGY AND SIDECAR RUNTIME                               |
+---------------------------------------------------------------------------------------------------+

 ┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
 │ CONTROL PLANE / INFRASTRUCTURE NODES: CSI Controller (Deployment - 2 Replicas)                  │
 │                                                                                                 │
 │  ┌────────────────────────────────────────┐  gRPC (Unix Domain Socket)                         │
 │  │ Kubernetes CSI Helper Sidecars:        │                                                     │
 │  │  ├── csi-provisioner (Watches PVCs)    │───────┐                                             │
 │  │  ├── csi-attacher    (Watches Volume-  │       │                                             │
 │  │  │                    Attachments)     │       │                                             │
 │  │  ├── csi-resizer     (Watches PVC size)│       │                                             │
 │  │  └── csi-snapshotter (Watches Snapshots│       │                                             │
 │  └────────────────────────────────────────┘       │                                             │
 │                                                   ▼                                             │
 │  ┌───────────────────────────────────────────────────────────────────────────────────────────┐  │
 │  │ Storage Vendor CSI Controller Container (e.g., aws-ebs-csi-driver)                        │  │
 │  │ Implements: CreateVolume, DeleteVolume, ControllerPublishVolume (Attach), ControllerExpand │  │
 │  └────────────────────────────────────────┬──────────────────────────────────────────────────┘  │
 └───────────────────────────────────────────┼─────────────────────────────────────────────────────┘
                                             │ Calls Cloud Provider Storage API
                                             ▼ (e.g., ec2:CreateVolume, ec2:AttachVolume)
                               ┌───────────────────────────┐
                               │ Cloud Storage Engine      │
                               │ (AWS EBS / GCE Persistent)│
                               └─────────────┬─────────────┘
                                             │ Physical Block Attachment to Host
                                             ▼
 ┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
 │ WORKER NODES: CSI Node Plugin (DaemonSet - 1 Pod Per Physical Node)                             │
 │                                                                                                 │
 │  ┌───────────────────────────────────────────────────────────────────────────────────────────┐  │
 │  │ node-driver-registrar Container: Registers Unix domain socket with local Kubelet plugin   │  │
 │  │ directory (/var/lib/kubelet/plugins_registry/)                                            │  │
 │  └────────────────────────────────────────┬──────────────────────────────────────────────────┘  │
 │                                           │ gRPC Socket                                         │
 │                                           ▼                                                     │
 │  ┌───────────────────────────────────────────────────────────────────────────────────────────┐  │
 │  │ Storage Vendor CSI Node Container (e.g., aws-ebs-csi-driver node agent)                  │  │
 │  │ Implements: NodeStageVolume (format filesystem: mkfs.ext4), NodePublishVolume (bind mount)│  │
 │  └───────────────────────────────────────────────────────────────────────────────────────────┘  │
 └─────────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 3. The 4 Standard CSI Helper Sidecars Explained

Kubernetes SIG Storage provides standard open-source helper sidecars that bridge Kubernetes API events to vendor gRPC calls:

1. **`csi-provisioner`:** Watches for newly created PersistentVolumeClaims referencing the StorageClass. When detected, it invokes the CSI gRPC call `CreateVolume()`. When a PVC is deleted, it calls `DeleteVolume()`.
2. **`csi-attacher`:** Watches Kubernetes `VolumeAttachment` API objects generated by the Kube-Scheduler. It calls the CSI gRPC method `ControllerPublishVolume()` to attach the physical cloud disk to the worker node's virtual machine instance.
3. **`csi-resizer`:** Watches for capacity increases in PVC manifests and triggers `ControllerExpandVolume()` on the cloud API so volume sizes expand without manual disk recreation.
4. **`node-driver-registrar`:** Runs inside the worker node DaemonSet. It announces the vendor CSI socket location to the local Kubelet, allowing the Kubelet to directly call the node driver via gRPC.

---

## 4. The Complete End-to-End Volume Lifecycle (gRPC RPC Flow)

When an application Pod is scheduled and starts running with persistent storage, the volume traverses a deterministic four-phase mounting pipeline:

```text
+---------------------------------------------------------------------------------------------------+
|                           THE 4-STAGE VOLUME MOUNTING LIFECYCLE                                   |
+---------------------------------------------------------------------------------------------------+

  STAGE 1: CreateVolume
  Triggered By: csi-provisioner
  Action: Cloud API allocates the raw block device (e.g., 50GB gp3 EBS disk created).
  Status: Disk exists in cloud inventory but is not connected to any computer.
       │
       ▼
  STAGE 2: ControllerPublishVolume (Attach)
  Triggered By: csi-attacher / AttachDetachController
  Action: Cloud API attaches the disk to the worker node VM instance (e.g., /dev/xvdf on Worker-01).
  Status: Raw unformatted block device is visible to Linux kernel on the worker machine.
       │
       ▼
  STAGE 3: NodeStageVolume (Format & Global Mount)
  Triggered By: Kubelet via CSI Node Plugin
  Action: Kubelet formats the raw block device with the requested filesystem (e.g., mkfs.ext4 /dev/xvdf)
          and mounts it to a global directory:
          /var/lib/kubelet/plugins/kubernetes.io/csi/pv/<pv-name>/globalmount
       │
       ▼
  STAGE 4: NodePublishVolume (Pod Bind Mount)
  Triggered By: Kubelet via CSI Node Plugin
  Action: Performs a Linux bind-mount from the global directory directly into the Pod's private
          container namespace directory:
          /var/lib/kubelet/pods/<pod-uuid>/volumes/kubernetes.io~csi/<pv-name>/mount
       │
       ▼
  CONTAINER RUNTIME STARTS APPLICATION CONTAINER! Data is accessible at /data inside Pod.
```

### Complete Tear-Down Lifecycle (When Pod is Deleted or Migrated)
When a Pod is terminated or rescheduled to another machine, the process executes in reverse:
1. **`NodeUnpublishVolume`:** Unmounts the bind-mount directory from the deleted container's namespace.
2. **`NodeUnstageVolume`:** Unmounts the global mount path from the node's `/var/lib/kubelet/` directory.
3. **`ControllerUnpublishVolume` (Detach):** Detaches the cloud block device from the worker node VM instance.
4. **`DeleteVolume` (if ReclaimPolicy is Delete):** Deletes the underlying cloud block volume permanently.

---

## 5. Failure Modes and Operational Debugging

### Scenario A: Volume Fails at `ControllerPublishVolume` with Multi-Attach Error
- **Symptom:** Pod stuck in `ContainerCreating`. `kubectl describe pod` outputs:
  `Warning FailedAttachVolume: Multi-Attach error for volume "pvc-xyz" Volume is already exclusively attached to one node and cannot be attached to another`
- **Underlying Cause:** A Pod using a `ReadWriteOnce` volume was rescheduled from Node A to Node B (e.g., Node A crashed). Cloud APIs enforce strict single-attachment rules on block devices. If Node A has not finished releasing the volume attachment, Node B is denied attachment permissions.
- **Remediation:** Verify if Node A is in `NotReady` state. If Node A is dead, force detach the volume or verify that the `VolumeAttachment` API object reflects the true state:
  ```bash
  kubectl get volumeattachment
  kubectl describe volumeattachment <attachment-id>
  ```

### Scenario B: Volume Fails at `NodeStageVolume` Due to Corrupted Filesystem
- **Symptom:** Kubelet event reports `MountVolume.SetUp failed for volume "pvc-xyz" : rpc error: code = Internal desc = format failed: exit status 1`.
- **Underlying Cause:** The block storage device contains incompatible partition tables or existing corrupted filesystems that `mkfs.ext4` refuses to overwrite without an explicit overwrite flag.
- **Investigation Command:** Check the CSI node driver logs directly on the affected worker node:
  ```bash
  kubectl logs -n kube-system daemonset/aws-ebs-csi-node -c ebs-plugin
  ```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - StorageClasses and Dynamic Provisioning](./02-StorageClasses-and-Dynamic-Provisioning.md) | [Index](../../../README.md) | [04 - Access Modes ReadWriteOnce ReadWriteMany and Block Volumes →](./04-Access-Modes-ReadWriteOnce-ReadWriteMany-and-Block-Volumes.md) |
