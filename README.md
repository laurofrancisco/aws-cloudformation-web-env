# AWS CloudFormation Baseline: VPC, EC2 & S3

An Infrastructure as Code (IaC) repository that provisions a standardized, secure, and ready-to-use AWS environment containing a custom VPC, a public-facing EC2 Web Server running Apache, and an Amazon S3 bucket.

This project was built to practice automated cloud provisioning, networking architecture, and infrastructure documentation.

## 🏗️ Architecture Blueprint

The CloudFormation template automates the creation of the following resources:

* **Networking:**
    * 1 Custom VPC (Default: `10.0.0.0/16`).
    * 1 Public Subnet associated with an Internet Gateway (IGW).
    * Route Tables configured to route external traffic (`0.0.0.0/0`) through the IGW.
* **Compute:**
    * 1 EC2 Instance (`t3.micro`) using the latest Amazon Linux 2 AMI.
    * Automated `UserData` script that installs, enables, and starts an Apache Web Server (`httpd`) hosting a basic HTML page.
    * `WaitCondition` implemented to guarantee the stack only finishes creation after the web server successfully bootstraps.
* **Security:**
    * 1 Security Group restricting traffic: Inbound allowed for **HTTP (Port 80)** and **SSH (Port 22)** from anywhere.
* **Storage:**
    * 1 Amazon S3 Bucket for general-purpose object storage.

---

## ⚙️ Parameters

| Parameter Name | Type | Default Value | Description |
| :--- | :--- | :--- | :--- |
| `LabVpcCidr` | String | `10.0.0.0/20` | CIDR Block for the VPC |
| `PublicSubnetCidr` | String | `10.0.0.0/24` | CIDR Block for the Public Subnet |
| `AmazonLinuxAMIID` | AWS SSM Parameter | *Dynamic* | Always fetches the latest Amazon Linux 2 AMI |
| `KeyName` | String | `default-lab-key` | SSH KeyPair name to access the EC2 instance |

---

## 🚀 How to Deploy

### Prerequisites
* An active AWS Account.
* An SSH Key Pair created in your preferred region (matching the `KeyName` parameter).
* AWS CLI installed and configured (Optional, for CLI deployment).

### Option 1: Via AWS Management Console
1. Log in to the AWS Console and navigate to **CloudFormation**.
2. Click **Create stack** -> **With new resources (standard)**.
3. Select **Upload a template file** and upload the `template.yaml` from this repository.
4. Fill in the parameters (ensure the `KeyName` matches an existing key pair in your account).
5. Advance through the steps and click **Submit**.

### Option 2: Via AWS CLI
Run the following command in your terminal:
```bash
aws cloudformation create-stack \
  --stack-name aws-web-baseline-lab \
  --template-body file://template.yaml \
  --parameters ParameterKey=KeyName,ParameterValue=YOUR_KEY_PAIR_NAME \
  --capabilities CAPABILITY_IAM
``` 

## 📊 Outputs
Once the stack status is CREATE_COMPLETE, you can check the Outputs tab to retrieve:

BucketName: The dynamically generated name of your S3 Bucket.

PublicIP: The Public IP address of your EC2 Web Server. You can paste this IP into your browser to see the "Hello from your web server!" message.

🚀 Developed as part of my cloud engineering and DevOps continuous learning journey.