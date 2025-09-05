# AWS-Terraform

🚀 AWS Infrastructure Deployment with Terraform

This project provisions AWS resources using Terraform.  
It automates the setup of infrastructure such as VPC, EC2 instances, Security Groups, and networking components, with optional provisioning scripts for application deployment.

---
 📑 Table of Contents
1. Project Overview
2. Architecture
3. Project Structure
4. Usage
5. Outputs
6. Clean Up

---

 📝 Project Overview
- Tool: Terraform  
- Cloud: AWS  
- Purpose: Automate infrastructure deployment (EC2, VPC, networking, etc.)

---

🏗 Architecture
The Terraform code provisions:
- VPC with public & private subnets  
- Internet Gateway + Route Tables
- EC2 Instances 
---

Terraform codes:

Connecting to AWS Credentials Using Provider Block and aws configure Command.

1. Provider Block in Code:
Install AWS CLI and SDK.
Create AWS Access Key and Secret Key.
Configure AWS CLI using `aws configure`.
Use the AWS SDK to create a "provider block" in your code, specifying the credentials and region.

2. AWS Configure Command:
Install AWS CLI.
Run `aws configure` and input Access Key, Secret Key, default region, and output format when prompted.
Folder Structure
aws-infra-deployment/
│── provider.tf
│── instance.tf
│── main.tf

providers.tf
provider "aws" {
  region     = "us-west-2"
}

Run Terraform init
instance.tf
resource "aws_instance" "Windows" 
{ 
ami = Copy instance id from AWS 
instance_type = "t3.micro"  tags = { Name = "Windows" }
}
Run
Terraform Plan
Terraform Apply
main.tf
 1 : Create a VPC
resource "aws_vpc" "myvpc"{
    cidr_block = "10.0.0.0/16"
    tags = {
        Name = "MyVPC"
    }
}

 2: Create a public subnet
resource "aws_subnet" "PublicSubnet"{
    vpc_id = aws_vpc.myvpc.id
    availability_zone = "us-east-1a"
    cidr_block = "10.0.1.0/24"
}

 3 : create a private subnet
resource "aws_subnet" "PrivSubnet"{
    vpc_id = aws_vpc.myvpc.id
    cidr_block = "10.0.2.0/24"
    map_public_ip_on_launch = true

}

 4 : create IGW
resource "aws_internet_gateway" "myIgw"{
    vpc_id = aws_vpc.myvpc.id
}

 5 : route Tables for public subnet
resource "aws_route_table" "PublicRT"{
    vpc_id = aws_vpc.myvpc.id
    route {
        cidr_block = "0.0.0.0/0"
        gateway_id = aws_internet_gateway.myIgw.id
    }
}

 6 : route table association public subnet 
resource "aws_route_table_association" "PublicRTAssociation"{
    subnet_id = aws_subnet.PublicSubnet.id
    route_table_id = aws_route_table.PublicRT.id
}


🔧 Prerequisites
- Terraform : https://developer.hashicorp.com/terraform/downloads installed  
- AWS CLI https://docs.aws.amazon.com/cli/ installed & configured  
- AWS IAM User with programmatic access (`Access Key + Secret Key`)  
- Key Pair for EC2 (to SSH into the instance)  

 
▶️ Usage

1. Clone the repository
bash
git clone https://github.com/your-username/aws-terraform-project.git
cd aws-terraform-project

2. Initialize Terraform
terraform init

3. Validate configuration
terraform validate

4. Plan infrastructure
terraform plan

5. Apply changes
terraform apply


📤 Outputs
Terraform will output values such as:
EC2 Public IP
VPC ID
Subnet IDs
Internet Gateway
Route Table



🧹 Clean Up
To destroy all resources:
terraform destroy -auto-approve

