Markdown
# Lab Title: End-to-End DevOps & GitOps Deployment on AWS

## Lab Objective
This lab introduces advanced DevOps and GitOps methodologies by implementing a fully automated, end-to-end deployment pipeline for a containerized application on AWS.
The goal is to provision the underlying infrastructure using Infrastructure as Code (Terraform), manage container orchestration using Kubernetes (Amazon EKS), automate the build process using Continuous Integration (GitHub Actions), and implement pull-based continuous deployment using GitOps (ArgoCD) as deployed in figure 1.

<img width="1248" height="647" alt="end to end infra last update" src="https://github.com/user-attachments/assets/66e0a677-00c2-443f-8112-aadf64d767e1" />


By the end of this lab, the student should be able to design, automate, and deploy a modern cloud-native architecture, bridging the gap between infrastructure provisioning and application delivery.

## Prerequisites
Students are expected to have:
* Basic knowledge in networking
* Basic knowledge in linux
* Basic knowledge in Git and GitHub commands such as push , commit , add status etc..

## Lab Scenario
This task requires students to create a small web page using AI or already created web page from GitHub and deploy full CICD pipelines and gitops on AWS. Here is the interactive Learning Roadmap. It is designed so that the trainee learns the theoretical concept first, then immediately applies it to your project to see the results firsthand.

The architecture should include the following workflows and components: Infrastructure Layer (Provisioned via Terraform)
* A highly available VPC with Public and Private Subnets.
* An Amazon EKS cluster for container orchestration.
* An isolated Amazon RDS (MySQL) database residing in a private subnet.
* An Amazon ECR repository for private Docker image storage.
* An EC2 instance acting as a Self-Hosted CI Runner.

### Continuous Integration (CI) Layer
* GitHub Actions triggered by code pushes to build the Docker image, push it to ECR, and automatically update the Kubernetes image tags in a separate manifests repository.

### Continuous Deployment (CD) Layer
* ArgoCD deployed within the EKS cluster to monitor the Kubernetes manifests repository and automatically synchronize (deploy) changes to the cluster using GitOps principles.
* An Application Load Balancer (ALB) routed via an EKS Ingress Controller to expose the application to the internet.

## Lab Tasks

### Phase 1: Application Containerization (Docker)
Before deploying the app to the cloud, the trainee needs to learn how to isolate it and make it run anywhere.

**What You Will Learn (Theory):**
* What is Docker? What is the difference between Containers and Virtual Machines (VMs)?
* How to write a Dockerfile.
* Essential Docker commands: build, image, run, tag.

**Hands-on Application (The Project):**
1. Create an application using AI or use an open source application from GitHub
2. Write a custom Dockerfile for it.
3. Build the Docker Image and run it on your local machine to ensure the application works perfectly inside the container.

### Phase 2: Cloud Computing Basics (AWS)
Now, the trainee must understand where this application will live and how its network is secured.

**What You Will Learn (Theory):**
* Networking (VPC): What is a Virtual Private Cloud? What is the difference between Public and Private Subnets?
* How do Security Groups work?
* Core AWS Services:
  * EC2: Virtual Servers.
  * NAT gateway
  * ALB: to ensure the traffic will reach multi target group
  * RDS: Managed Relational Databases.
  * ECR: Elastic Container Registry (Private Docker repo).
  * EKS: Elastic Kubernetes Service.
  * Route 53 DNS

**Hands-on Application (The Project):**
1. Open the Project Architecture Diagram and analyze it: Why did we place the RDS database in a private subnet?
2. Why is the Application Load Balancer (ALB) acting as the public-facing gateway?

### Phase 3: Infrastructure as Code (Terraform - IaC)
Instead of clicking through the AWS console, the trainee will learn how to program the cloud.

**What You Will Learn (Theory):**
* Core Terraform concepts: Providers, Resources, Variables, and the State File.
* How to write declarative code to provision basic AWS resources.

**Hands-on Application (The Project):**
Write the Terraform code to provision the entire project infrastructure from scratch:
1. Provision the VPC (Subnets, Route Tables, Internet Gateways).
2. Provision an EC2 instance (which will later be used as the CI Runner).
3. Provision the RDS (MySQL) database.
4. Provision the ALB
5. Provision the ECR repository for the application images.
6. Provision the Amazon EKS cluster.
7. Buy a domain name from namecheap to provision it on the infrastructure.
8. Run `terraform apply` and watch your cloud infrastructure build itself automatically.

### Phase 4: Container Orchestration (Kubernetes)
With the cloud and EKS cluster ready, the trainee needs to tell Kubernetes how to run the application.

**What You Will Learn (Theory):**
* Kubernetes architecture basics (Nodes, Pods).
* Writing configuration files (Manifests) in YAML format (Deployments, Services).
* Routing internet traffic to the application (Ingress & Load Balancers).

**Hands-on Application (The Project):**
1. Create a new GitHub repository and name it K8s-Manifests.
2. Write a Deployment manifest to manage the Flask App pods.
3. Write a Service manifest for internal networking.
4. Install and configure the AWS Load Balancer Controller inside the EKS cluster to link Kubernetes with the AWS ALB.

### Phase 5: Continuous Integration (CI - GitHub Actions)
It is time to connect the source code to the cloud automatically.

**What You Will Learn (Theory):**
* The concept of Continuous Integration (CI).
* How Workflows, Jobs, and Steps operate in GitHub Actions.
* Read about LINT and SAST tests to perform on the code in parallel.
* The difference between GitHub-Hosted Runners and Self-Hosted Runners.

**Hands-on Application (The Project):**
1. SSH into the EC2 instance created in Phase 3 and configure it as a Self-Hosted Runner.
2. Write a Workflow file (.yml) in the app repository that triggers on every git push to perform the following:
   * Build a new Docker image for the app.
   * Push the image to the Amazon ECR repository.
   * Clone the K8s-Manifests repository, update the Image Tag to the newly built version, and push the changes back to GitHub.

### Phase 6: Continuous Deployment & GitOps (CD - ArgoCD)
The final phase implements modern deployment practices to close the loop.

**What You Will Learn (Theory):**
* What is the GitOps methodology? How does it differ from traditional CI/CD pipelines?
* How ArgoCD works as a "watcher" for Git repositories.

**Hands-on Application (The Project):**
1. Install ArgoCD inside the Amazon EKS cluster.
2. Link ArgoCD to the K8s-Manifests GitHub repository.
3. Enable the Auto-Sync feature. Now, whenever the GitHub Actions pipeline updates the image tag in the repository, ArgoCD will detect the change, pull the update, and deploy it to the EKS cluster instantly.

### Phase 7: The Moment of Truth (End-to-End Test & Clean Up)
* **The Final Test:** Modify a single word in the Flask App source code and run `git push`. Sit back and watch the CI pipeline build and push the image, then watch ArgoCD automatically sync and deploy it. Open the ALB URL to verify the change is live.
* **Resource Teardown (Clean Up):** Run `terraform destroy` to tear down all AWS resources and avoid unnecessary cloud billing.

## Expected Learning Outcomes
After completing this lab, students should be able to:
* Implement Infrastructure as Code using Terraform for complex architectures.
* Configure and manage self-hosted runners for continuous integration.
* Automate container builds and registry uploads using GitHub Actions.
* Implement GitOps methodology for continuous deployment using ArgoCD.
* Expose Kubernetes services to the internet securely using Ingress and Load Balancers.
الخطوة 2: إنشاء ملف README.md الرئيسي
اضغط كليك يمين في أي مساحة فاضية باللون الأسود تحت مجلد teerraform_proj عشان تتأكد إنك بتعمل الملف بالمكان الرئيسي (مش جوا مجلد فرعي).

اختار New File وسميه README.md.

انسخ هذا الكود والصقه فيه واحفظه (Ctrl + S):

Markdown
# 🚀 End-to-End GitOps & Cloud-Native CI/CD Pipeline

## 📋 Project Overview

![end to end infra](https://private-user-images.githubusercontent.com/235390932/602512096-f124ec24-85e7-4c74-abfa-2cb7023eed02.png)
This project demonstrates a fully automated, production-ready CI/CD pipeline and cloud-native infrastructure provisioning. Following strict **GitOps** principles, the architecture is decoupled into separate repositories. It leverages GitHub Actions for Continuous Integration (CI) and ArgoCD for Continuous Deployment (CD), ensuring seamless, declarative deployments to an Amazon EKS cluster.

## 🗂️ Project Repositories

| Component | Repository Link | Description |
| --- | --- | --- |
| **1. Application Code** | [mo-nseirat/to-do-list-app](https://github.com/mo-nseirat/to-do-list-app) | Contains the Flask application source code, Dockerfile, and GitHub Actions CI workflow. |
| **2. Infrastructure (IaC)** | [mo-nseirat/eks_terraform_infrastructure_on_AWS](https://github.com/mo-nseirat/eks_terraform_infrastructure_on_AWS) | Contains Terraform configurations to automatically build the entire AWS production cloud infrastructure. |
| **3. Kubernetes Manifests** | [mo-nseirat/eks-k8s-manifests](https://github.com/mo-nseirat/eks-k8s-manifests) | Contains K8s deployment, service (ClusterIP), and Ingress manifests for ArgoCD to pull and sync. |

## 🛠️ Tech Stack & Tools

* **Cloud Provider:** AWS (EKS, EC2, ECR, RDS, ALB)
* **Infrastructure as Code:** HashiCorp Terraform
* **Containerization:** Docker
* **Orchestration:** Kubernetes (Amazon EKS)
* **CI/CD & GitOps:** GitHub Actions, ArgoCD
* **Application & DB:** Flask, MySQL (Amazon RDS)

## ⚙️ Architecture Workflow

1. **Infrastructure Provisioning:** Terraform automates and builds the entire AWS environment (VPC, EKS, EC2 Self-Hosted Runner, ECR, and RDS in an isolated private subnet).
2. **Continuous Integration (CI):**
	* Developer pushes code to the GitHub repository.
	* GitHub Actions triggers the pipeline and executes the job on an EC2 Self-Hosted Runner.
	* The runner builds and tags the Docker image, then pushes it to Amazon ECR (Private Docker Registry).
	* GitHub Actions automatically updates the Kubernetes image tag in the K8s Manifests repository.
3. **Continuous Deployment (CD):**
	* ArgoCD detects changes and pulls the updated K8s manifests.
	* ArgoCD syncs and deploys the new state to the Amazon EKS Cluster (Flask App Pods).
4. **Traffic Routing & Database:**
	* User traffic (Port 443) hits the AWS Application Load Balancer (ALB).
	* ALB routes traffic to the K8s Ingress, which forwards it to the Flask App Pods.
	* The Flask application connects to the MySQL Amazon RDS database via Port 3306.
