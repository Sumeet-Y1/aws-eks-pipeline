# 🚀 AWS EKS Pipeline

A Spring Boot Hello World application deployed on **AWS EKS** using **Jenkins CI/CD pipeline** with full Infrastructure as Code via Terraform.

## 🏗️ Architecture

```
Developer pushes code to GitHub
        ↓
GitHub sends webhook to Jenkins (EC2)
        ↓
Jenkins pulls latest code
        ↓
Jenkins builds Docker image
        ↓
Jenkins pushes image to AWS ECR
        ↓
Jenkins deploys to AWS EKS (Kubernetes)
        ↓
App is live behind AWS LoadBalancer
```

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| **Spring Boot** | Java application |
| **Docker** | Containerization |
| **Jenkins** | CI/CD pipeline (self-hosted on EC2) |
| **AWS ECR** | Docker image registry |
| **AWS EKS** | Kubernetes cluster |
| **Terraform** | Infrastructure as Code |
| **Helm** | Kubernetes package manager |

## 📁 Project Structure

```
aws-eks-pipeline/
├── src/                        # Spring Boot application
├── k8s/                        # Kubernetes manifests
│   ├── deployment.yaml
│   └── service.yaml
├── terraform/                  # AWS infrastructure
│   ├── main.tf
│   └── eks.tf
├── Dockerfile                  # Docker image definition
├── Jenkinsfile                 # CI/CD pipeline definition
└── README.md
```

## ⚙️ Prerequisites

- AWS Account with CLI configured
- Terraform installed
- kubectl installed
- Docker installed
- Java 21+
- Helm installed

## 🚀 Setup & Deployment

### 1. Clone the repo
```bash
git clone https://github.com/Sumeet-Y1/aws-eks-pipeline.git
cd aws-eks-pipeline
```

### 2. Provision AWS infrastructure with Terraform
```bash
cd terraform
terraform init
terraform apply
```

### 3. Connect kubectl to EKS
```bash
aws eks update-kubeconfig --region ap-south-1 --name aws-eks-pipeline
kubectl get nodes
```

### 4. Configure Jenkins
- Access Jenkins at `http://<ec2-ip>:8080`
- Connect GitHub repo via SCM
- Add GitHub webhook for auto-triggering
- Run pipeline

### 5. Access the app
```bash
kubectl get svc
# Hit the external LoadBalancer IP in your browser
```

## 🔄 CI/CD Pipeline Stages

```
Checkout → Build Docker Image → Push to ECR → Deploy to EKS
```

Every `git push` to main automatically triggers the full pipeline via GitHub webhook.

## 📊 Monitoring

Prometheus and Grafana monitoring stack is configured via Helm in the `monitoring` namespace.

> **Note:** Full monitoring stack requires nodes with at least **t3.medium** or higher. On **t3.micro**, Kubernetes system pods consume most available memory, leaving insufficient resources for the monitoring stack. Upgrade node instance type for production use.

```bash
# Install monitoring stack (requires t3.medium+ nodes)
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm install prometheus prometheus-community/prometheus --namespace monitoring --create-namespace
helm install grafana grafana/grafana --namespace monitoring
```

## 👨‍💻 Author

**Sumeet** — [@Sumeet-Y1](https://github.com/Sumeet-Y1)