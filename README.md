![Alt text](/Host_a_Dynamic_Website_on_AWS_with_EKS_upd.png)

## Overview
This project demonstrates how to host a dynamic website using AWS services, specifically focusing on Docker containers, Amazon EKS, Amazon ECR, and monitoring with Grafana and Prometheus. The architecture includes a three-tier VPC configuration with public and private subnets, NAT Gateways, and specific security settings to ensure scalability and security.

## Project Structure

The project consists of several components implemented in a structured manner:

1. **Infrastructure as Code**: 
   - Created all necessary AWS infrastructure using Terraform, including a three-tier VPC, Internet Gateway, NAT Gateways, public and private route tables.

2. **Source Code Management**:
   - Created two private repositories in GitHub: one for the Dockerfile and another for the RentZone application code. 
   - Synchronized both repositories with a local development environment.

3. **Docker Setup**:
   - Built a Docker image locally referencing the provided Dockerfile and RentZone app code.
   - Created an Amazon ECR repository and pushed the built Docker image using AWS CLI.

4. **Database Setup**:
   - Created an empty Amazon RDS database and migrated the provided SQL schema into it.

5. **Kubernetes Management**:
   - Developed a private GitHub repository to store Kubernetes manifest files and scripts for configuring and launching the EKS cluster. 
   - Synced this repository with a local development environment.

6. **Kubernetes Tools Installation**:
   - Installed necessary tools on the local machine and bastion host (Chocolatey, `kubectl`, `eksctl`, `helm`) for Kubernetes management.

7. **Secrets Management**:
   - Created secrets in AWS Secrets Manager, including environment variables used at runtime (e.g., database credentials).
   - Configured IAM policy for Secrets Manager to allow the EKS cluster to read the secrets.

8. **EKS Cluster Setup**:
   - Created EKS Cluster IAM roles and associated policies to enable cluster operation and node management.
   - Established a Node Group containing the underlying EC2 instances that support the EKS cluster.

9. **Application Deployment**:
   - Created Kubernetes manifests: 
     - **`secret-provider-class.yaml`**: Defines how the application consumes secrets from AWS Secrets Manager.
     - **`deployment.yaml`**: Describes the deployment of the RentZone application within the cluster.
     - **`service.yaml`**: Exposes the application through a Network Load Balancer (NLB) that operates within the private subnets.

10. **Access Configuration**:
    - Configured `kubectl` to interact with the EKS cluster and created a dedicated namespace to group resources logically.
    - Established how the EKS cluster accesses secrets stored in AWS Secrets Manager.
    - Associated an IAM OIDC Identity Provider with the EKS Cluster and created a service account for EKS.

11. **Monitoring Setup**:
    - Installed Grafana and Prometheus in the EKS cluster to monitor CPU and Memory utilization for all cluster resources.
    - Followed up by importing Grafana dashboards that provide CPU and Memory usage metrics.

12. **Bastion Host Access**:
    - Deployed a Windows 2022 EC2 instance as a bastion host in a private subnet, connecting to it using AWS Systems Manager (SSM) for administrative purposes.
    - Established an RDP session via an SSM tunnel from the local machine to the bastion host and accessed the private application hosted by the EKS cluster.

13. **Route 53**:
    - Registered the domain name and set up a DNS record using Route 53.

## Technologies Used
- **AWS Services**: EC2, RDS, EKS, ECR, Elastic Load Balancer, Systems Manager, Route 53
- **Containers**: Docker
- **Orchestration**: Kubernetes
- **Monitoring**: Grafana, Prometheus
- **Infrastructure as Code**: Terraform
- **Scripting**: PowerShell, bash scripts for deployment automation
