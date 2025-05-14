# README.md

## MERN Stack Blog App Deployment

This repository contains the deployment setup for a MERN stack blog application using Terraform and Ansible. The backend runs on an EC2 instance, the frontend is hosted on AWS S3, and MongoDB Atlas is used for the database.

### Table of Contents
- [Architecture](#architecture)
- [Deployment Steps](#deployment-steps)
- [Cleanup](#cleanup)

## Architecture

The architecture of the solution consists of:
- **EC2 Instance**: Hosts the backend application.
- **MongoDB Atlas**: Cloud database service for storing blog data.
- **S3 Buckets**: 
  - **Frontend Bucket**: Hosts the static website.
  - **Media Bucket**: Manages media uploads with IAM policies.

## Deployment Steps

### Prerequisites
1. **AWS Account**: Ensure you have an AWS account.
2. **Terraform**: Install Terraform.
3. **Ansible**: Install Ansible.
4. **Node.js**: Ensure Node.js is installed on your local machine.

### Steps

1. **Clone the Repository**
   ```bash
   git clone https://github.com/cw-barry/blog-app-MERN.git
   cd blog-app-MERN
   ```

2. **Set Up Terraform**
   - Navigate to the Terraform configuration files and initialize:
   ```bash
   cd terraform
   terraform init
   ```
   - Create resources:
   ```bash
   terraform apply
   ```

3. **Configure MongoDB Atlas**
   - Set up a MongoDB Atlas cluster.
   - Whitelist your EC2 instance's IP address.
   - Create a user and retrieve the connection string.

4. **Set Up Ansible**
   - Navigate to the Ansible directory:
   ```bash
   cd ansible
   ```
   - Update variables in the playbook files with your credentials.
   - Run the Ansible playbook:
   ```bash
   ansible-playbook backend-playbook.yml
   ```

5. **Frontend Deployment**
   - Navigate to the frontend directory:
   ```bash
   cd frontend
   ```
   - Create the `.env` file:
   ```bash
   cat > .env << EOF
   VITE_BASE_URL=http://<your-ec2-dns>:5000/api
   VITE_MEDIA_BASE_URL=https://<your-s3-media-bucket-name>.eu-north-1.amazonaws.com
   EOF
   ```
   - Configure AWS CLI:
   ```bash
   aws configure
   ```
   - Install dependencies and build the frontend:
   ```bash
   npm install -g pnpm@latest-10
   pnpm install
   pnpm run build
   ```
   - Deploy to S3:
   ```bash
   aws s3 sync dist/ s3://<your-s3-frontend-bucket-name>/
   ```

## Cleanup
To clean up all AWS resources created via Terraform, run:
```bash
terraform destroy
```
Remove any sensitive credentials from your EC2 instance and delete MongoDB Atlas users or IP access rules as necessary.

