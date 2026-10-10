# 05 - Drift Detection Strategies & Automation

## 1. What Is Infrastructure Drift and Why Is It Dangerous?

**Infrastructure Drift** is the phenomenon where the real, live state of cloud infrastructure diverges from the declared configuration stored in Git.

In high-velocity production environments, drift is almost always caused by **out-of-band modifications**:
1. **Emergency Midnight "ClickOps":** During a production outage, an on-call engineer logs directly into the AWS Console and modifies an EC2 security group rule to allow traffic, or manually doubles an RDS instance size. In the relief of resolving the incident, the engineer forgets to update the Terraform code in Git.
2. **Third-Party Automation & Autoscalers:** External cloud services (such as AWS Auto Scaling, AWS Backup, or automated RDS OS patching) modify resource attributes without Terraform's involvement.
3. **Malicious Actor Intervention:** An attacker gains unauthorized access and opens firewall ports, modifies IAM policies, or creates unauthorized compute instances.

### The Hidden Destructive Danger of Drift:
Drift is dangerous not only because it represents undocumented state, but because **it turns routine Terraform applies into catastrophic outages**:
- Suppose someone manually altered a database parameter in the AWS console.
- Three weeks later, an unrelated developer edits a tag in Terraform and runs `terraform apply`.
- Terraform compares the code against the drifted live state and determines: *“This parameter cannot be updated in-place; the database must be DESTROYED and RECREATED!”*
- **Result:** The routine apply destroys the production database!

```text
+---------------------------------------------------------------------------------------------------+
|                        THE SILENT DRIFT & DESTRUCTIVE APPLY CATASTROPHE                           |
+---------------------------------------------------------------------------------------------------+

 WEEK 1 (THE EMERGENCY HOTFIX):
 ┌────────────────────────────────────────────────────────┐
 │ Outage occurs ──► Engineer logs into AWS Web Console   │
 │ Manually changes DB instance class: db.t3.medium ──►   │
 │ db.r6g.xlarge. (Change is NOT committed to Git!)       │
 └────────────────────────────────────────────────────────┘
                             │
                             ▼
 WEEK 4 (THE ROUTINE RELEASE):
 ┌────────────────────────────────────────────────────────┐
 │ Junior Developer edits a tag in main.tf and applies.   │
 │ Terraform calculates delta against live drifted state: │
 │ Action: Demotes DB from r6g.xlarge BACK to t3.medium!  │
 │ Database reboots during peak business hours!           │
 └────────────────────────────────────────────────────────┘
```

---

## 2. Detection Mechanism 1: `terraform plan -refresh-only`

Modern Terraform (v1.0+) provides a safe, non-destructive command engineered specifically to detect and inspect drift without proposing any infrastructure modifications:

```bash
terraform plan -refresh-only
```

### How `-refresh-only` Works Under the Hood:
1. Terraform connects to cloud provider APIs and queries the live actual state of every resource in the state file.
2. It compares the live cloud state against the existing `terraform.tfstate`.
3. If drift is discovered, it outputs a clean preview showing only the out-of-band changes:
   ```text
   Note: Objects have changed outside of Terraform

   Terraform detected the following changes made outside of Terraform since the
   last "terraform apply" which may have affected this plan:

     # aws_security_group.web has changed:
       ~ ingress = [
           + {
               cidr_blocks = ["0.0.0.0/0"]
               from_port   = 22
               protocol    = "tcp"
               to_port     = 22
             },
         ]
   ```
4. If the changes are legitimate, running `terraform apply -refresh-only` updates the state file to match the cloud **without modifying a single cloud resource**.

---

## 3. The 3 Strategies for Drift Remediation

When drift is detected, platform engineering teams apply one of three remediation paths:

```text
+---------------------------------------------------------------------------------------------------+
|                        THE THREE DRIFT REMEDIATION PATHWAYS                                       |
+---------------------------------------------------------------------------------------------------+

                                    [ DRIFT DETECTED ]
                                             │
         ┌───────────────────────────────────┼───────────────────────────────────┐
         ▼                                   ▼                                   ▼
 STRATEGY 1: RECONCILE (OVERWRITE)   STRATEGY 2: ADOPT (UPDATE CODE)     STRATEGY 3: REVERT
 Overwrite rogue cloud changes with  Legitimize emergency console changes Undo rogue changes
 clean Git code:                     by writing HCL in Git:              directly in cloud console
 "terraform apply"                   Submit PR ──► Merge ──► Plan = 0    or AWS CLI.
```

1. **Reconcile (Forced Overwrite):** The rogue change was unauthorized or dangerous. Running standard `terraform apply` overwrites the cloud configuration and resets it back to the authoritative Git declaration.
2. **Adopt (Update Source Code):** The emergency change was legitimate and necessary. Engineers write matching HCL in Git, verify that `terraform plan` shows 0 changes, and merge the Pull Request.
3. **Revert (Manual Reversal):** Immediately delete rogue resources or undo dangerous security group modifications via cloud APIs.

---

## 4. Production CI/CD Drift Detection Pipeline (GitHub Actions)

In mature enterprise operations, drift detection runs on an automated schedule (e.g., every 4 hours) using the **`-detailed-exitcode`** flag:

```yaml
# File: .github/workflows/drift-detection.yml
name: Scheduled Infrastructure Drift Detection

on:
  schedule:
    # Run automatically every 4 hours Monday through Friday
    - cron: '0 */4 * * 1-5'
  workflow_dispatch: # Enable manual on-demand trigger

jobs:
  detect-drift:
    name: Scan Production State for Drift
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read
      issues: write

    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Configure AWS Credentials (OIDC)
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: arn:aws:iam::123456789012:role/GitHubActionsDriftDetector
          aws-region: us-east-1

      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v3
        with:
          terraform_version: 1.8.2

      - name: Initialize Backend
        run: terraform init

      - name: Execute Drift Detection
        id: plan
        run: |
          # Exit Code 0 = Clean (No Drift)
          # Exit Code 1 = Fatal Error
          # Exit Code 2 = Drift Detected!
          set +e
          terraform plan -detailed-exitcode -no-color > drift_report.txt
          EXIT_CODE=$?
          echo "exit_code=$EXIT_CODE" >> $GITHUB_OUTPUT
          
          if [ $EXIT_CODE -eq 2 ]; then
            echo "DRIFT DETECTED!"
            exit 2
          elif [ $EXIT_CODE -eq 1 ]; then
            echo "TERRAFORM EXECUTION FAILED!"
            exit 1
          else
            echo "INFRASTRUCTURE IS FULLY SYNCHRONIZED."
            exit 0
          fi

      - name: Alert Engineering via Slack
        if: steps.plan.outputs.exit_code == '2'
        uses: slackapi/slack-github-action@v1.25.0
        with:
          payload: |
            {
              "text": "🚨 *CRITICAL: Infrastructure Drift Detected in Production!*",
              "blocks": [
                {
                  "type": "section",
                  "text": {
                    "type": "mrkdwn",
                    "text": "*Alert:* Production AWS infrastructure has diverged from Git!\n*Workflow:* <${{ github.server_url }}/${{ github.repository }}/actions/runs/${{ github.run_id }}|View GitHub Actions Run>"
                  }
                }
              ]
            }
        env:
          SLACK_WEBHOOK_URL: ${{ secrets.SLACK_DEVOPS_ALERTS_WEBHOOK }}
```

---

## 5. Event-Driven Real-Time Drift Detection with CloudTrail and EventBridge

While scheduled polling runs every few hours, security-critical organizations implement **real-time event-driven drift detection**:

```text
+---------------------------------------------------------------------------------------------------+
|                        REAL-TIME EVENT-DRIVEN DRIFT DETECTION ARCHITECTURE                        |
+---------------------------------------------------------------------------------------------------+

 1. Rogue Action: Admin modifies Security Group in AWS Console (AuthorizeSecurityGroupIngress)
        │
        ▼
 2. AWS CloudTrail records API event with user identity and IP address
        │
        ▼
 3. Amazon EventBridge catches event pattern: "eventName": "AuthorizeSecurityGroupIngress"
    Filter: Exclude calls made by Terraform CI/CD IAM Role!
        │
        ▼
 4. Triggers AWS Lambda / Slack Webhook within 30 SECONDS!
    Optionally triggers an automated AWS Step Function to REVERT the unauthorized rule instantly!
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Integration Testing Terratest and Kitchen Terraform](./04-Integration-Testing-Terratest-and-Kitchen-Terraform.md) | [Index](../../../README.md) | [06 - Policy as Code Sentinel OPA and Conftest →](./06-Policy-as-Code-Sentinel-OPA-and-Conftest.md) |
