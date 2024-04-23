# BookStoreIaaC

### Project Description:

CI/CD Pipeline Implementation for **.NET Application** using **Dockerized solution** and **ECS**

### Overview:

Implemented CI/CD pipeline for a **.NET** app automating build and deployment processes using containerized solution on **AWS ECS** and **AWS ECR**.

### Technical Details:

1. Application overview:
   The application has a three-tier architecture.

   - Frontend: Blazor Web App, .Net8
   - Backend: ASP.NET Core Web API, .Net8
   - Database: MSSQL server 2022

2. CI/CD Pipeline:

   - Tools Used: Jenkins on an EC2 instance orchestrated the CI/CD pipeline.
   - Build Triggers: Jenkins automatically triggers builds upon push event, ensuring code consistency. Each commit triggers a specific Jenkins pipeline based on the commit message format.
     For example: to initiate a build for the backend on the DEV enviroment, include [DEV-BACKEND] in the commit message.
     Template: [*env_name*-*app_layer*]
   - Build Process:
     - **Retrieve Code from Git**
     - **Build Docker Image**: Jenkins initiates the Docker image building process with the Dockerfile, a set of instructions for building the container image.
     - **Image Security Check with Trivy**: After the Docker image is built, the Trivy plugin scans the Docker image for known vulnerabilities If Trivy detects critical level of vulnerabilities in the Docker image, Jenkins fails the build process. The build fails to prevent the deployment of potentially insecure containers into production. Jenkins provides a detailed report of the vulnerabilities found by Trivy.
     - **Push Image to Amazon ECR (Elastic Container Registry)**: Once the Docker image is built, Jenkins pushes it to Amazon ECR. Tags such as the build number and commit ID are added to the image for versioning and traceability purposes.
     - **Backup to Docker Hub**: As a backup measure, Jenkins also pushes the Docker image to Docker Hub.
     - **Trigger Terraform Build:** Jenkins triggers the Terraform build process.Terraform is used to define and provision the infrastructure resources, such as the ECS service and task definitions. It ensures that the required infrastructure is in place to deploy and run the Dockerized application.
   - Deployment Strategy:
     - Artifact Versioning: Each built image has specific tags (buil number and commit ID) for traceability and potential rollback.
     - Rolling Deployment: new **ECS task definitions** are gradually rolled out to the **ECS service**.It allows for a controlled, gradual transition from the old version of the task to the new one. This helps ensure that the service remains available and responsive throughout the deployment process.

3. Infrastructure Management:

   - **Terraform Provisioning**: used Terraform for infrastructure-as-code, ensuring consistent, scalable environments.
   - **Terraform Cloud Integration**: Utilized Terraform Cloud to securely store sensitive information and configuration variables, enhancing security and facilitating centralized management.

4. Cloud Networking Info:

![Alt text](Docker_Networking.png?raw=true "Title")

5. CI/CD Diagram:

![Alt text](Docker_CICD.png?raw=true "Title")

Note:

- The DB base image is used as a base image for the DB image
- The ASPNET base image is used as a base image for Frontend & Backend images.

6. Configuration management process

- The configuration management process was automated. When the user closes a **Pull Request** and merges changes to the **'docker' branch**, the **GitHub Action** workflow is being executed.
- It sends request to the **AWS API Gateway** that triggers **AWS Lambda** function which execute **AWS System Manager Commands**.
- These commands execute **Ansible playbooks** that finally manage the configuration state on Jenkins.
- In addition, all system logs and monitoring information go to **AWS CloudWatch**.

![Alt text](JenkinsCaaC_process.png?raw=true "Title")

### Achievements:

- Implemented CI/CD for pipeline for three-layer application with **Dockerized solution** and **ECS** using **Jenkins**
- Utilizing script pipelines for autimated workflows and efficiency.
- Configured **Git** & **Jenkins** to trigger build pipelines based on the specific message format
- Built customer docker images using Dockerfile and pushed them to **AWS ECR** (primary repository) **Docker Hub** (backup repository)
- Scaned built images for security vulnerabilities using **Trivy** plugin
- using **Terraform & Terraform Cloud** provisioned **EC2, Security groups, ELB, Target group, ECS, VPC Endpoints, IAM, VPC, SSM documents, CloudWatch Log group, API Gateway** as IaaC
- Designed Terraform infrastructure code with modular constructs, enabling easy scalability to other environments beyond dev and prod
- Integrated **GitHub Actions**, **AWS API Gateway**, **AWS Lambda**, and **AWS System Manager Commands**, enhancing the automation in configuration management process.
- Utilized **Ansible playbooks** to manage configuration state on Jenkins
- Using **AWS CloudWatch**, implemented monitoring and logging of EC2 instance on which Jenkins is run with such metrics as CPU, memory utilization, disk space used percentage and network throughput.
- Designed and created diagrams such as networking diagram, CI/CD diagram & configuration management process diagram
