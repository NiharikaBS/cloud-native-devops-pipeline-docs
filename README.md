# Cloud-native-devops-pipeline-docs
Complete DevOps execution steps for deploying the cloud-native-devops-pipeline project on AWS using Terraform, EKS, GitHub Actions, and Argo CD.

# 📘 Documentation for cloud-native-devops-pipeline

This repo contains the complete step-by-step execution guide for deploying the cloud-native-devops-pipeline on AWS.

---

## 🚀 Project Overview

- Tools used: **Terraform**, **AWS EKS**, **GitHub Actions**, **Argo CD**, **Kubernetes**, **Docker**, **ALB**, etc.

🔗 Main application code: [cloud-native-devops-pipeline repo](https://github.com/NiharikaBS/cloud-native-devops-pipeline)

---

## Architecture Diagram
```mermaid
graph TD
subgraph Service Diagram
accounting(Accounting):::dotnet
ad(Ad):::java
cache[(Cache<br/>&#40Valkey&#41)]
cart(Cart):::dotnet
checkout(Checkout):::golang
currency(Currency):::cpp
email(Email):::ruby
flagd(Flagd):::golang
flagd-ui(Flagd-ui):::typescript
fraud-detection(Fraud Detection):::kotlin
frontend(Frontend):::typescript
frontend-proxy(Frontend Proxy <br/>&#40Envoy&#41):::cpp
image-provider(Image Provider <br/>&#40nginx&#41):::cpp
load-generator([Load Generator]):::python
payment(Payment):::javascript
product-catalog(Product Catalog):::golang
quote(Quote):::php
recommendation(Recommendation):::python
shipping(Shipping):::rust
queue[(queue<br/>&#40Kafka&#41)]:::java
react-native-app(React Native App):::typescript

ad ---->|gRPC| flagd

checkout -->|gRPC| cart
checkout --->|TCP| queue
cart --> cache
cart -->|gRPC| flagd

checkout -->|gRPC| shipping
checkout -->|gRPC| payment
checkout --->|HTTP| email
checkout -->|gRPC| currency
checkout -->|gRPC| product-catalog

fraud-detection -->|gRPC| flagd

frontend -->|gRPC| ad
frontend -->|gRPC| cart
frontend -->|gRPC| checkout
frontend ---->|gRPC| currency
frontend ---->|gRPC| recommendation
frontend -->|gRPC| product-catalog

frontend-proxy -->|gRPC| flagd
frontend-proxy -->|HTTP| frontend
frontend-proxy -->|HTTP| flagd-ui
frontend-proxy -->|HTTP| image-provider

Internet -->|HTTP| frontend-proxy

load-generator -->|HTTP| frontend-proxy

payment -->|gRPC| flagd

queue -->|TCP| accounting
queue -->|TCP| fraud-detection

recommendation -->|gRPC| product-catalog
recommendation -->|gRPC| flagd

shipping -->|HTTP| quote

react-native-app -->|HTTP| frontend-proxy
end

classDef dotnet fill:#178600,color:white;
classDef cpp fill:#f34b7d,color:white;
classDef golang fill:#00add8,color:black;
classDef java fill:#b07219,color:white;
classDef javascript fill:#f1e05a,color:black;
classDef kotlin fill:#560ba1,color:white;
classDef php fill:#4f5d95,color:white;
classDef python fill:#3572A5,color:white;
classDef ruby fill:#701516,color:white;
classDef rust fill:#dea584,color:black;
classDef typescript fill:#e98516,color:black;
```

```mermaid
graph TD
subgraph Service Legend
  dotnetsvc(.NET):::dotnet
  cppsvc(C++):::cpp
  golangsvc(Go):::golang
  javasvc(Java):::java
  javascriptsvc(JavaScript):::javascript
  kotlinsvc(Kotlin):::kotlin
  phpsvc(PHP):::php
  pythonsvc(Python):::python
  rubysvc(Ruby):::ruby
  rustsvc(Rust):::rust
  typescriptsvc(TypeScript):::typescript
end

classDef dotnet fill:#178600,color:white;
classDef cpp fill:#f34b7d,color:white;
classDef golang fill:#00add8,color:black;
classDef java fill:#b07219,color:white;
classDef javascript fill:#f1e05a,color:black;
classDef kotlin fill:#560ba1,color:white;
classDef php fill:#4f5d95,color:white;
classDef python fill:#3572A5,color:white;
classDef ruby fill:#701516,color:white;
classDef rust fill:#dea584,color:black;
classDef typescript fill:#e98516,color:black;
```

## 📁 Execution steps 

| Step  | Description                                                  | Folder                                                                                                   |
| ----- | ------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------- |
| 1️⃣   | Set up AWS resources (S3 and DynamoDB) for Terraform backend | [`01-aws-setup/`](./01-aws-setup)                                                                        |
| 2️⃣   | Containerize the microservices (Go, Python, Java)            | [`02-Containerization/`](./02-Containerization)                                                          |
| 🖥️ 3 | Run the project locally using Docker Compose                 | [`03-run-locally/`](./03-run-locally)                                                                    |
| 4️⃣   | Provision VPC, EKS, and other infrastructure using Terraform | [`04-terraform-vpc-s3-eks/`](./04-terraform-vpc-s3-eks)                                                  |
| 5️⃣   | Configure `kubeconfig` to access the EKS cluster             | [`05-configure-eks-kubeconfig/`](./05-configure-eks-kubeconfig)                                          |
| 6️⃣   | Deploy applications and services on Kubernetes               | [`06-deploy-on-kubernetes/`](./06-deploy-on-kubernetes)                                                  |
| 7️⃣   | Set up and configure ALB Ingress Controller                  | [`06-deploy-on-kubernetes/05-lb-ingress-controller`](./06-deploy-on-kubernetes/05-lb-ingress-controller) |
| 8️⃣   | Configure CI/CD pipelines using GitHub Actions and Argo CD   | [`07-cicd/`](./07-cicd)                                                                                  |


---

## 🛠 Tools Used

- **AWS** (EKS, S3, DynamoDB, IAM, CloudWatch)
- **Terraform** (Infrastructure as Code)
- **Kubernetes** (Helm, Ingress, Services)
- **GitHub Actions** (CI)
- **Argo CD** (CD)
- **Docker**, **OpenTelemetry**

---

## 📸 Screenshots 


- Argo CD UI
- GitHub Actions run
- Services in AWS/EKS
- LoadBalancer access

---



