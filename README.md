# Terraform-AWS-Infrastructure-Web-Hosting-via-ALB-with-S3-Integration

This project automates the deployment of a highly available web application environment on AWS using Terraform. It provisions a custom VPC with subnets, security groups, EC2 instances running a web server, a Load Balancer (ALB), and an S3 bucket.

🧱 Infrastructure Overview
Here’s what this Terraform configuration sets up:

🔹 VPC with two public subnets

🔹 Internet Gateway and routing

🔹 Security Group for HTTP and SSH access

🔹 2 EC2 Instances running a custom HTML page using user_data

🔹 Application Load Balancer (ALB) to distribute traffic

🔹 S3 Bucket with a unique name for future static storage or backup

🧰 Prerequisites
Ensure the following are installed and configured:

Terraform

AWS CLI

An AWS account with permissions to manage EC2, VPC, ALB, and S3

📁 Project Structure
bash
Copy
Edit
terraform-aws-web-hosting/
├── main.tf              # Terraform config for all resources
├── variables.tf         # Input variables (optional)
├── outputs.tf           # Output values (like ALB DNS)
├── userdata.sh          # Script to install web server and serve HTML (Instance 1)
├── userdata1.sh         # Script for Instance 2
└── README.md            # You're reading it!
🛠️ What This Project Does
1. VPC and Subnets
Creates a custom VPC (10.0.0.0/16) with two public subnets across AZs us-east-1a and us-east-1b.

2. Internet Gateway & Routing
Enables internet access by associating public subnets with a route table pointing to an Internet Gateway.

3. Security Group
Allows inbound:

HTTP (port 80) from anywhere

SSH (port 22) from anywhere
Outbound: All traffic allowed

4. EC2 Instances
Deploys two EC2 instances using Amazon Linux AMI (ami-0731becbf832f281e) in each public subnet.
Each instance:

Installs a web server via user_data.sh / userdata1.sh

Hosts a simple HTML page

5. Application Load Balancer
Distributes HTTP traffic across both EC2 instances for high availability.

6. S3 Bucket
Creates a uniquely named S3 bucket for storage (subhankar-devops-bucket-20250609-bglr-xxxxxx).

🚀 How to Deploy
1. ✅ Initialize Terraform
bash
Copy
Edit
terraform init
2. 🔍 Preview Resources
bash
Copy
Edit
terraform plan
3. 🚀 Apply the Configuration
bash
Copy
Edit
terraform apply
Confirm with yes to proceed.

🌐 Access Your Website
Once provisioning is complete, Terraform will output your Load Balancer DNS:

bash
Copy
Edit
loadbalancerdns = myalb-xxxxxxxx.elb.amazonaws.com
Open this DNS URL in your browser to see the hosted HTML page.

📂 User Data (Web Server Script)
Your userdata.sh might look like this:

bash
Copy
Edit
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Welcome to DevOps</h1>" > /var/www/html/index.html
userdata1.sh can host a different message for Instance 2.

🧹 Cleanup
To destroy all resources:

bash
Copy
Edit
terraform destroy
🔒 Notes
Use IAM roles or policies if accessing S3 from EC2 in future versions

Make sure your key-pair and ssh access are properly set for manual login

For production, consider using HTTPS, Auto Scaling, and CloudFront with S3
