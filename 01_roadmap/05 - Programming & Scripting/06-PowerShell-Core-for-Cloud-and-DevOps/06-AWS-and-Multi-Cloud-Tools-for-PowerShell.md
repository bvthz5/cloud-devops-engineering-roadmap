# 06 - AWS and Multi-Cloud Tools for PowerShell

## 1. `AWS.Tools` Modular Architecture

Historically, AWS provided the monolithic `AWSPowerShell.NetCore` module, which loaded hundreds of megabytes of DLLs into memory. Modern DevOps practices mandate the **`AWS.Tools.*`** modular suite, allowing developers to import only the specific AWS service modules required.

```powershell
# Install only needed services
Install-Module -Name AWS.Tools.Common -Force
Install-Module -Name AWS.Tools.S3 -Force
Install-Module -Name AWS.Tools.EC2 -Force
Install-Module -Name AWS.Tools.SecurityToken -Force
```

---

## 2. Cross-Account IAM Role Assumption in PowerShell

In enterprise environments, engineers authenticate against a bastion identity account and assume target roles in Workload, Staging, or Production accounts using AWS Security Token Service (STS).

```powershell
function Connect-AwsTargetAccount {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory = $true)]
        [string]$TargetRoleArn,

        [Parameter(Mandatory = $true)]
        [string]$SessionName = "DevOpsAutomationSession",

        [string]$Region = "us-east-1"
    )

    Write-Verbose "Requesting STS temporary credentials for role: $TargetRoleArn"
    
    $stsResponse = Use-STSRole -RoleArn $TargetRoleArn -RoleSessionName $SessionName -Region $Region

    # Set active session credentials globally in PowerShell session
    Set-AWSCredential `
        -AccessKey $stsResponse.Credentials.AccessKeyId `
        -SecretKey $stsResponse.Credentials.SecretAccessKey `
        -SessionToken $stsResponse.Credentials.SessionToken `
        -StoreAs "ActiveTargetSession"

    Set-DefaultAWSRegion -Region $Region
    Write-Host "Successfully assumed $TargetRoleArn. Session valid until $($stsResponse.Credentials.Expiration)" -ForegroundColor Green
}
```

---

## 3. Automated S3 Security Scanner: Detecting Public Buckets

```powershell
[CmdletBinding()]
param()

$allBuckets = Get-S3Bucket

foreach ($b in $allBuckets) {
    $bucketName = $b.BucketName
    
    # Check Public Access Block Configuration
    try {
        $pab = Get-S3PublicAccessBlock -BucketName $bucketName -ErrorAction Stop
        $isBlocked = $pab.PublicAccessBlockConfiguration.BlockPublicAcls -and `
                     $pab.PublicAccessBlockConfiguration.BlockPublicPolicy -and `
                     $pab.PublicAccessBlockConfiguration.IgnorePublicAcls -and `
                     $pab.PublicAccessBlockConfiguration.RestrictPublicBuckets

        if (-not $isBlocked) {
            Write-Warning "Bucket '$bucketName' has INCOMPLETE Public Access Block settings!"
        } else {
            Write-Verbose "Bucket '$bucketName' is securely locked down."
        }
    } catch [Amazon.S3.AmazonS3Exception] {
        if ($_.ErrorCode -eq 'NoSuchPublicAccessBlockConfiguration') {
            Write-Error "SECURITY CRITICAL: Bucket '$bucketName' has NO Public Access Block configured!"
        } else {
            Write-Warning "Could not evaluate bucket '$bucketName': $($_.Exception.Message)"
        }
    }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Azure PowerShell Az Module and Automation](./05-Azure-PowerShell-Az-Module-and-Automation.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
