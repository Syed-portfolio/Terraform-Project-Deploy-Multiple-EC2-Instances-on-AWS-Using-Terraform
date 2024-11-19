# Terraform-workshop

Here are the complete step-by-step instructions that you can copy and paste into a Word document or PDF for your project:

---

# Project: Deploy Multiple EC2 Instances on AWS Using Terraform

### Prerequisites

- **AWS Account**: Ensure you have an active AWS account.
- **SSH Key Pair**: Create an SSH key pair in your AWS account and download the `.pem` file.
- **Amazon Linux Instance**: Ensure you have an Amazon Linux instance running with access to install and run Terraform.

### Step 1: Install Terraform on Amazon Linux

1. **Update Package Repository**:
   ```sh
   sudo yum update -y
   ```

2. **Download and Unzip Terraform**:
   ```sh
   wget https://releases.hashicorp.com/terraform/1.0.11/terraform_1.0.11_linux_amd64.zip
   unzip terraform_1.0.11_linux_amd64.zip -d /usr/local/bin
   ```

3. **Verify Terraform Installation**:
   ```sh
   terraform -version
   ```

### Step 2: Configure AWS CLI

1. **Install AWS CLI (if not already installed)**:
   ```sh
   sudo yum install aws-cli -y
   ```

2. **Configure AWS CLI**:
   ```sh
   aws configure
   ```
   - Enter your AWS Access Key ID, Secret Access Key, Default region name (`us-east-2`), and Default output format (e.g., `json`).

### Step 3: Create a Terraform Configuration File

1. **Create a Project Directory**:
   ```sh
   mkdir aws-multi-server
   cd aws-multi-server
   ```

2. **Create and Edit `main.tf`**:
   ```sh
   nano main.tf
   ```

3. **Add the Following Configuration to `main.tf`**:

   ```hcl
   provider "aws" {
     region = "us-east-2"
   }

   resource "aws_security_group" "allow_ssh" {
     vpc_id = "vpc-07c013bc2e179830d"  # Your VPC ID

     ingress {
       from_port   = 22
       to_port     = 22
       protocol    = "tcp"
       cidr_blocks = ["0.0.0.0/0"]
     }
   }

   resource "aws_instance" "web" {
     count         = 3
     ami           = "ami-0942ecd5d85baa812"  # Your AMI ID
     instance_type = "t2.micro"
     subnet_id     = "subnet-0b313bf9dfbc70934"  # Your Subnet ID
     vpc_security_group_ids = [aws_security_group.allow_ssh.id]

     tags = {
       Name = "WebServer-${count.index + 1}"
     }
   }
   ```

4. **Save and Exit**:
   - In Nano, press `Ctrl + O`, `Enter`, then `Ctrl + X` to save and exit the editor.

### Step 4: Initialize and Apply the Terraform Configuration

1. **Initialize Terraform**:
   ```sh
   terraform init
   ```

2. **Plan the Deployment**:
   ```sh
   terraform plan
   ```

3. **Apply the Configuration**:
   ```sh
   terraform apply
   ```
   - When prompted to confirm, type `yes` and press `Enter`.

### Step 5: Verify the Deployment

1. **Check AWS Management Console**:
   - Go to the **EC2 Dashboard** in the AWS Management Console.
   - Verify that three EC2 instances have been created, each tagged as `WebServer-1`, `WebServer-2`, and `WebServer-3`.

2. **SSH into Instances**:
   - Use the public IP addresses of the instances to SSH into them from your PuTTY session. For example:
     ```sh
     ssh -i "C:\Users\nomii\Downloads\Syed1.pem" ec2-user@<public-ip-of-instance>
     ```
   - Replace `<public-ip-of-instance>` with the actual public IP address of each instance.

---

By following these steps, you'll be able to deploy multiple EC2 instances using Terraform on your Amazon Linux instance. This guide provides a complete, step-by-step process to set up and deploy your infrastructure!!
