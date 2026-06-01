# 🚀 AWS EKS Pipeline

A Spring Boot Hello World application deployed on **AWS EKS** using **Jenkins CI/CD pipeline** with full Infrastructure as Code via Terraform and real-time monitoring via Prometheus & Grafana.

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
| **Helm** | Kubernetes package manager |
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

Full monitoring stack deployed via Helm on the EKS cluster using **kube-prometheus-stack** — includes Prometheus, Grafana, and AlertManager.

```bash
# Install monitoring stack
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install monitoring prometheus-community/kube-prometheus-stack --namespace monitoring --create-namespace --timeout 10m
```

### Access Grafana
```bash
# Get admin password
$encoded = kubectl get secret --namespace monitoring monitoring-grafana -o jsonpath="{.data.admin-password}"
[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encoded))

# Port forward
kubectl --namespace monitoring port-forward svc/monitoring-grafana 3000:80
```

Open `http://localhost:3000` → login with `admin` and the password above.

### Grafana Dashboard
Import dashboard ID **`15661`** for full Kubernetes cluster monitoring:
- Node CPU & Memory usage
- Pod health and status
- Network traffic overview
- Namespace resource breakdown

> **Note:** kube-prometheus-stack requires nodes with sufficient resources. Tested on **m7i-flex.large** (2 vCPU, 8GB RAM).

## 👨‍💻 Author

**Sumeet** — [@Sumeet-Y1](https://github.com/Sumeet-Y1)