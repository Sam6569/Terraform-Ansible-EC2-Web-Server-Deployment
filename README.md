# Terraform-Ansible EC2 Web Server Deployment

Automated AWS EC2 web server deployment using Terraform for infrastructure provisioning and Ansible for configuration management.

## Overview

This project demonstrates Infrastructure as Code (IaC) by:
- **Terraform**: Provisions AWS EC2 instance with security groups and SSH key pairs
- **Ansible**: Configures the instance with Nginx web server and custom content

## Architecture

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│  Terraform  │───▶│   AWS EC2   │◀───│   Ansible   │
│ (Provision) │    │ (Instance)  │    │ (Configure) │
└─────────────┘    └─────────────┘    └─────────────┘
```

## Prerequisites

- AWS CLI configured with credentials
- Terraform >= 1.0
- Ansible >= 2.9
- SSH key pair (`~/.ssh/id_rsa` and `~/.ssh/id_rsa.pub`)

## Quick Start

### 1. Deploy Infrastructure

```bash
# Initialize Terraform
terraform init

# Plan deployment
terraform plan

# Apply configuration
terraform apply
```

### 2. Configure Server

```bash
# Get EC2 public IP
terraform output ec2_public_ip

# Create Ansible inventory
cat > inventory.yml << EOF
all:
  hosts:
    webserver:
      ansible_host: <EC2_PUBLIC_IP>
      ansible_user: ec2-user
      ansible_ssh_private_key_file: ~/.ssh/id_rsa
EOF

# Run Ansible playbook
ansible-playbook -i inventory.yml webserver.yml
```

### 3. Access Web Server

Open browser: `http://<EC2_PUBLIC_IP>`

## Project Structure

```
├── main.tf          # AWS provider, key pair, security group
├── ec2.tf           # EC2 instance and outputs
├── .gitignore       # Terraform state files exclusion
└── README.md        # This file
```

## Resources Created

- **EC2 Instance**: t2.micro Amazon Linux 2023
- **Security Group**: SSH (22) and HTTP (80) access
- **Key Pair**: For SSH authentication

## Configuration

### AWS Provider
- Region: `us-east-1`
- Profile: `Sam` (update in `main.tf`)

### Security Group Rules
- Inbound: SSH (22), HTTP (80)
- Outbound: All traffic allowed

## Cleanup

```bash
terraform destroy
```

## Notes

- Ensure SSH key permissions: `chmod 600 ~/.ssh/id_rsa`
- AMI ID is region-specific (currently set for us-east-1)
- Update AWS profile name in `main.tf` as needed

## License

MIT License