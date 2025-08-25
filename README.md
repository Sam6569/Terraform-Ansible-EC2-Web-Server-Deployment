# Terraform-Ansible-EC2-Web-Server-Deployment
This project demonstrates how to **provision an AWS EC2 instance using Terraform** and **configure it automatically using Ansible**. By the end, you will have a live web server running **Nginx** with a custom HTML page.

## Features

- Provision EC2 instance with Terraform
  - Create key pairs
  - Set up security groups (SSH and HTTP access)
- Configure EC2 instance with Ansible
  - Install and start Nginx
  - Deploy a custom HTML page
- Fully automated workflow (no manual server configuration required)

## Prerequisites

- [AWS CLI](https://aws.amazon.com/cli/)
- Terraform installed (v1.x recommended)
- Ansible installed (v2.x recommended)
- AWS account with proper IAM permissions
- WSL or Linux/Mac environment (Windows users need WSL for proper key permissions)

## Setup

### 1. Terraform

1. Clone the repository:
   ```bash
   git clone <your-repo-url>
   cd <repo-directory>
   
Initialize Terraform:

terraform init

Apply Terraform to create resources:

terraform apply

Note the EC2 public IP from Terraform output:

terraform output ec2_public_ip

2. Ansible
Create inventory.yaml with the public IP of your EC2:

all:

  hosts:
  
    webserver:
    
      ansible_host: <EC2_PUBLIC_IP>
      
      ansible_user: ec2-user
      
      ansible_ssh_private_key_file: ~/.ssh/id_rsa
      
Run the Ansible playbook:

ansible-playbook -i inventory.yaml webserver.yml

Verify in your browser:


http://<EC2_PUBLIC_IP>

Notes
Ensure your private SSH key has proper permissions:


chmod 600 ~/.ssh/id_rsa

This project uses Amazon Linux 2023, so yum/dnf is used instead of apt.

Project Structure

├── terraform/

│   ├── main.tf

│   ├── ec2.tf

├── ansible/

│   ├── inventory.yaml

│   └── webserver.yaml

└── README.md

Workflow Diagram

Terraform (local)

      │
      
      ▼
      
  AWS EC2 Instance
  
      │
      
      ▼
      
 Ansible (local) → Configures EC2 → Installs Nginx & Deploys HTML
 
      │
      
      ▼
      
  Browser Access → Live Web Page
  
Learning Points
Combining Terraform (infrastructure as code) with Ansible (configuration management)

Handling SSH key permissions in WSL

Understanding package management differences between Amazon Linux 2 and 2023

Deploying a live web server in AWS automatically

License
MIT License



