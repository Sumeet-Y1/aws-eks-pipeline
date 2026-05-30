# 🚀 AWS EKS Pipeline

A Spring Boot Hello World application deployed on **AWS EKS** using **Jenkins CI/CD pipeline** with **Prometheus & Grafana** monitoring.

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
Prometheus scrapes metrics from EKS
        ↓
Grafana visualizes dashboards & alerts
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
| **Prometheus** | Metrics collection |
| **Grafana** | Monitoring dashboards |

## 📁 Project Structure

```
aws-eks-pipeline/
├── src/                        # Spring Boot application
├── k8s/                        # Kubernetes manifests
│   ├── deployment.yaml
│   └── service.yaml
├── terraform/                  # AWS infrastructure
│   ├── main.tf
│   ├── eks.tf
│   └── ec2-jenkins.tf
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

### 3. Configure Jenkins
- Access Jenkins at `http://<ec2-ip>:8080`
- Connect GitHub repo
- Add AWS credentials
- Run pipeline

### 4. Access the app
```bash
kubectl get svc
# Hit the external IP in your browser
```

## 📊 Monitoring

- **Prometheus** → scrapes metrics from EKS cluster
- **Grafana** → visualize at `http://<grafana-ip>:3000`

## 👨‍💻 Author

**Sumeet** — [@Sumeet-Y1](https://github.com/Sumeet-Y1)