# BookStore

**Project Description**: CI/CD Pipeline Implementation for Multi-Environment **.NET** Application

**Overview**:
Implemented CI/CD pipeline for a **.NET** app automating build and deployment for DEV and PROD environments.

**Technical Details**:

1. Application overview:
   The application has a three-tier architecture.

   - Frontend: Blazor Web App, .Net8
   - Backend: ASP.NET Core Web API, .Net8
   - Database: MSSQL server 2022

2. CI/CD Pipeline:

   - Tools Used: **Jenkins** on an EC2 instance orchestrated the CI/CD pipeline for both environments.
   - Build Process: Jenkins automatically triggers builds upon push event, ensuring code consistency. Each commit triggers a specific Jenkins pipeline based on the commit message format.
     For example: to initiate a build for the backend on the DEV enviroment, include [DEV-BACKEND] in the commit message.
     Template: [*env_name*-*app_layer*]
   - Deployment Strategy:
     - Artifact Versioning: Each build has a specific timestamp for traceability and potential rollback.
     - Rolling Deployment: Updates are applied to one **EC2 instance** at a time, ensuring a controlled rollout without service interruption.

3. Infrastructure Management:

   - Terraform Provisioning: used **Terraform** for infrastructure-as-code, ensuring consistent, scalable environments.
   - Terraform Cloud Integration: Utilized **Terraform Cloud** to securely store sensitive information and configuration variables, enhancing security and facilitating centralized management.
   - Backups:
     - DB: Utilized **AWS RDS** autobackups for automated, regular backups of the database, ensuring data integrity and recovery options.
     - EC2: Execution of a custom runbook within **AWS SSM Automation** that creates AMI containing a specific build. This way it provides a reliable backup mechanism for the entire server infrastructure.
   - CloudWatch Integration: Application logs were sent to **AWS CloudWatch** for centralized monitoring and analysis, ensuring swift issue resolution.

4. Cloud Networking Info:

   - Low cost
     ![Alt text](Networking_LOW_COST.png?raw=true "Title")
   - Regular
     ![Alt text](Networking_Ideal.png?raw=true "Title")

5. CI/CD Diagram:

![Alt text](CICD.png?raw=true "Title")

6. Jenkins plugins used
   - SSH Agent
   - .NET SDK Support

**Achievements**:

- Implemented CI/CD for pipelines DEV & PROD for three-layer application deployment using **Jenkins**
- Installing and updating the Jenkins plugins to achieve CI/CD
- Managed Jenkins administration tasks to optimize CI/CD pipelines
- Utilizing script pipelines for autimated workflows and efficiency. Implemented dynamic value retrieval using AWS CLI, avoiding hardcoded values for enhanced flexibility and security
- Configured **Git & Jenkins** to trigger build pipelines based on the specific message format
- using **Terraform & Terraform Cloud** provisioned **EC2, Security groups, Auto-scaling, Load balancers (ELBs), Elastic beanstalk, S3, RDS, IAM, VPC** as IaaC
- Designed Terraform infrastructure code with modular constructs, enabling easy scalability to other environments beyond dev and prod
- Backup configuation and management flow both for **AWS RDS and EC2**
- Integrated **AWS CloudWatch** for centralized monitoring and analysis of application logs
- Designed and created various diagrams such as networking diagram, CI/CD diagram both for low cost and regular solutions
