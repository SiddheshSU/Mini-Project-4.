# Enterprise Student Record and Academic Management Platform

## Project Overview

The **Enterprise Student Record and Academic Management Platform** is a cloud infrastructure automation project developed using **Amazon Web Services (AWS), Terraform, and Ansible**.

The main objective of this project is to automate the provisioning, configuration, and deployment of a Student Record Management application on AWS. Instead of manually configuring servers, this project uses Infrastructure as Code (IaC) and configuration management tools to create a consistent and repeatable deployment process.

The application manages student records, course enrollment, and academic marks through a Flask-based web application.

## Objectives

- Automate AWS infrastructure provisioning using Terraform.
- Configure and manage servers using Ansible.
- Deploy a containerized Flask application using Docker.
- Create a secure and isolated AWS network.
- Configure Nginx as a reverse proxy.
- Automate application deployment and server configuration.
- Validate application health using API smoke tests.
- Reduce manual configuration errors and configuration drift.

## Technologies Used

| Technology | Purpose |
|---|---|
| Amazon Web Services (AWS) | Cloud infrastructure |
| Terraform | Infrastructure as Code |
| Ansible | Server configuration automation |
| Docker | Application containerization |
| Python | Application development |
| Flask | REST API development |
| Nginx | Web server and reverse proxy |
| Ubuntu 22.04 LTS | Operating system |
| SQLite | Database storage |
| Git and GitHub | Version control and project hosting |

## AWS Infrastructure Architecture

The project uses the following AWS resources:

- **Amazon EC2:** Hosts the application and Docker runtime.
- **Amazon VPC:** Provides an isolated virtual network.
- **Public Subnet:** Hosts the EC2 instance.
- **Internet Gateway:** Enables internet connectivity.
- **Route Table:** Manages network traffic routing.
- **Security Group:** Controls inbound and outbound traffic.
- **Amazon S3:** Stores backups and supports infrastructure state storage when configured.

### Network Configuration

| Component | Configuration |
|---|---|
| VPC CIDR | `10.1.0.0/16` |
| Public Subnet CIDR | As defined in Terraform configuration |
| EC2 Instance | `t2.micro` |
| Operating System | Ubuntu 22.04 LTS |
| SSH Port | `22` |
| HTTP Port | `80` |
| Application Port | `8080` |

**Note:** The VPC CIDR and ports above follow the student project requirements in the assignment. The actual configuration should match the Terraform files in this repository.

## Project Workflow

The deployment follows these steps:

1. **Infrastructure Provisioning:** Terraform creates the AWS networking resources, security group, EC2 instance, and other configured resources.
2. **Server Configuration:** Ansible connects to the EC2 instance through SSH.
3. **Dependency Installation:** Ansible installs the required system packages and Docker.
4. **Application Deployment:** Docker runs the Student Record Management application.
5. **Web Server Configuration:** Nginx forwards incoming HTTP requests to the application.
6. **Application Validation:** API smoke tests verify that the application is running correctly.

### Workflow Diagram

```text
Developer
   |
   v
Terraform
   |
   v
AWS Infrastructure
   |
   +---- VPC and Subnet
   |
   +---- Security Group
   |
   +---- EC2 Instance
   |
   v
Ansible Configuration
   |
   +---- Install Docker
   |
   +---- Configure Nginx
   |
   +---- Deploy Application
   |
   v
Docker Container
   |
   v
Student Record Flask API
   |
   v
API Health Checks
```

## Repository Structure

The following is a suggested repository structure. Adjust the names to match your actual files.

```text
student-record-platform/
│
├── terraform/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   ├── provider.tf
│   └── terraform.tfvars.example
│
├── ansible/
│   ├── inventory.ini
│   ├── playbook.yml
│   └── roles/
│       ├── common/
│       ├── docker/
│       ├── nginx/
│       └── student_app/
│
├── app/
│   ├── app.py
│   ├── requirements.txt
│   └── Dockerfile
│
├── tests/
│   └── test_api.py
│
├── .gitignore
└── README.md
```

## Application Features

The platform is designed to support the following academic management operations:

- Student record management.
- Course enrollment management.
- Academic marks management.
- REST API access to student information.
- Application health monitoring.
- Containerized application deployment.

The actual features depend on the Flask application implemented in the repository.

## API Endpoints

The assignment specifies the following endpoints for validation.

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/api/health` | Check application health |
| GET | `/api/students` | Retrieve student records |
| POST | `/api/marks` | Submit academic marks |

**Expected result:** The health endpoint should return a successful response when the application is running correctly.

The exact request format and response structure depend on the application code.

## Prerequisites

Before starting the deployment, ensure that you have:

- An AWS account with the required permissions.
- Terraform installed.
- Ansible installed on the control machine.
- AWS CLI configured with appropriate credentials.
- An SSH key pair for EC2 access.
- Git installed.
- Docker knowledge for application containerization.

## Installation and Deployment

### Step 1: Clone the Repository

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd student-record-platform
```

Replace `<YOUR_GITHUB_REPOSITORY_URL>` with your actual GitHub repository URL.

### Step 2: Configure AWS Credentials

Configure the AWS CLI using an authorized IAM identity.

```bash
aws configure
```

Enter the required AWS access information when prompted.

**Security:** Never upload AWS access keys, secret keys, private SSH keys, or other credentials to GitHub.

### Step 3: Provision Infrastructure with Terraform

Navigate to the Terraform directory.

```bash
cd terraform
```

Initialize Terraform:

```bash
terraform init
```

Validate the configuration:

```bash
terraform validate
```

Preview the infrastructure changes:

```bash
terraform plan
```

Create the AWS resources:

```bash
terraform apply
```

Review the execution plan and approve the deployment when prompted.

### Step 4: Configure the EC2 Instance with Ansible

Update the Ansible inventory with the public IP address of the EC2 instance and the correct SSH key location.

Example inventory:

```ini
[student_servers]
student-host ansible_host=<EC2_PUBLIC_IP> ansible_user=ubuntu
```

Replace `<EC2_PUBLIC_IP>` with the actual EC2 public IP address.

Test the SSH connection:

```bash
ansible -i ansible/inventory.ini student_servers -m ping
```

Run the configuration playbook from the repository root:

```bash
ansible-playbook -i ansible/inventory.ini ansible/playbook.yml
```

The playbook should install the required dependencies, configure the web server, and deploy the application according to the implemented roles.

### Step 5: Verify the Application

Check the application health endpoint:

```bash
curl http://<EC2_PUBLIC_IP>/api/health
```

Replace `<EC2_PUBLIC_IP>` with the actual public IP address.

If the deployment is successful and the health endpoint is configured as expected, the application should return a successful health response.

## Security Considerations

- Follow the principle of least privilege when configuring AWS IAM permissions.
- Restrict SSH access to trusted IP addresses.
- Expose only the ports required by the application.
- Do not commit credentials, `.pem` files, or Terraform state files to GitHub.
- Use encryption and appropriate access controls for S3 backups.
- Use HTTPS and TLS certificates for production deployments.
- Review Terraform plans before applying infrastructure changes.
- Configure application and database backups where required.

## Cleanup

To remove the AWS resources managed by Terraform when they are no longer required, run:

```bash
cd terraform
terraform destroy
```

Review the proposed changes before confirming.

**Important:** Destroying infrastructure may interrupt the application and remove cloud resources. Back up any important data first. S3 buckets containing objects may require additional cleanup depending on their configuration.

## Learning Outcomes

Through this project, we learn how to:

- Implement Infrastructure as Code using Terraform.
- Automate server configuration using Ansible.
- Deploy containerized applications on AWS EC2.
- Configure virtual networks and security groups.
- Integrate Terraform outputs with Ansible inventory.
- Configure Nginx as a reverse proxy.
- Perform application health checks and API smoke tests.
- Apply DevOps automation principles to cloud infrastructure.

## Conclusion

The Enterprise Student Record and Academic Management Platform demonstrates how Terraform and Ansible can automate cloud infrastructure provisioning and application deployment on AWS.

By integrating AWS, Terraform, Ansible, Docker, Flask, and Nginx, the project provides a repeatable approach to deploying a student academic management application while reducing manual configuration and improving deployment consistency.

## Author

**Siddhesh Ashok Sonawane**

Engineering Student

**Project:** Enterprise Student Record and Academic Management Platform  
**Domain:** Cloud Computing and DevOps Automation
