# 🚀 End-to-End GitOps & Cloud-Native CI/CD Pipeline

## 📋 Project Overview
<img width="1248" height="647" alt="end to end infra last update" src="https://github.com/user-attachments/assets/f124ec24-85e7-4c74-abfa-2cb7023eed02" />

This project demonstrates a fully automated, production-ready CI/CD pipeline and cloud-native infrastructure provisioning. Following strict **GitOps** principles, the architecture is decoupled into separate repositories. It leverages GitHub Actions for Continuous Integration (CI) and ArgoCD for Continuous Deployment (CD), ensuring seamless, declarative deployments to an Amazon EKS cluster.

## 🗂️ Project Repositories
| Component | Repository Link | Description |
|-----------|-----------------|-------------|
| **1. Application Code** | https://github.com/mo-nseirat/to-do-list-app | Contains the Flask application source code, Dockerfile, and GitHub Actions CI workflow. |
| **2. Infrastructure (IaC)** | https://github.com/mo-nseirat/eks_terraform_infrastructure_on_AWS | Contains Terraform configurations to automatically build the entire AWS production cloud infrastructure. |
| **3. Kubernetes Manifests** |https://github.com/mo-nseirat/eks-k8s-manifests | Contains K8s deployment, service (ClusterIP), and Ingress manifests for ArgoCD to pull and sync. |

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
   - Developer pushes code to the GitHub repository.
   - GitHub Actions triggers the pipeline and executes the job on an EC2 Self-Hosted Runner.
   - The runner builds and tags the Docker image, then pushes it to Amazon ECR (Private Docker Registry).
   - GitHub Actions automatically updates the Kubernetes image tag in the K8s Manifests repository.
3. **Continuous Deployment (CD):** 
   - ArgoCD detects changes and pulls the updated K8s manifests.
   - ArgoCD syncs and deploys the new state to the Amazon EKS Cluster (Flask App Pods).
4. **Traffic Routing & Database:** 
   - User traffic (Port 443) hits the AWS Application Load Balancer (ALB).
   - ALB routes traffic to the K8s Ingress, which forwards it to the Flask App Pods.
   - The Flask application connects to the MySQL Amazon RDS database via Port 3306.
