# final_k8s_project

Final Kubernetes Project

Overview

This project demonstrates how to deploy a production-grade, scalable, and secure Node.js backend application with a MongoDB replica set using Kubernetes. It showcases best practices in containerization, security, database clustering, and infrastructure as code.

🧩 Architecture

Client sends requests to an NGINX LoadBalancer

LoadBalancer routes requests to one of the replicated Node.js pods

Node.js app communicates with a MongoDB replica set via a headless service

Configuration and secrets are managed securely via ConfigMaps and Secrets

Each MongoDB pod has its own Persistent Volume to retain data

Kubernetes Job initializes replica set and MongoDB user

Workloads are isolated within a Namespace with RBAC policies for security



📂 Project Structure

📦 final_k8s_project/
├── 📁 k8s/                           # Kubernetes manifests
│   ├── 📄 namespace.yaml            # Namespace definition
│   ├── 📄 configmap.yaml            # App config via ConfigMap
│   ├── 📄 secrets.yaml              # Sensitive data via Secret
│   ├── 📄 node-app-deployment.yaml  # Node.js app Deployment
│   ├── 📄 mongo-statefulset.yaml    # MongoDB replica set StatefulSet
│   ├── 📄 mongo-service.yaml        # MongoDB headless service
│   ├── 📄 mongo-init-job.yaml       # Job to init replica set & create user
│   └── 📄 rbac.yaml                 # RBAC roles and bindings
├── 📄 Dockerfile                    # Multi-stage Docker build
├── 📄 .dockerignore                 # Files to ignore in Docker build
├── 📄 .trivyignore                  # Trivy security scan ignore rules
├── 📄 README.md                     # Project documentation
├── 📁 src/                          # Node.js application source code
│   └── 📄 ...                       # Your .js files and logic
└── 📄 package.json                  # Node.js dependencies and scripts


🚀 Getting Started

Prerequisites

Kubernetes cluster (local via KinD, Minikube, or cloud)

kubectl configured

Docker

1. Clone the repository

git clone https://github.com/YoussefAzozz/final_k8s_project.git
cd final_k8s_project

2. Apply Kubernetes Manifests

kubectl apply -f k8s/namespace.yaml
kubectl apply -f k8s/configmap.yaml
kubectl apply -f k8s/secrets.yaml
kubectl apply -f k8s/mongo-service.yaml
kubectl apply -f k8s/mongo-statefulset.yaml
kubectl apply -f k8s/node-app-deployment.yaml
kubectl apply -f k8s/rbac.yaml

3. Initialize MongoDB Replica Set

kubectl apply -f k8s/mongo-init-job.yaml

This job will:

Initiate the replica set

Create the MongoDB user

Switch to the mongoose_test DB

🛡️ Security

RBAC: Fine-grained access control using Kubernetes Role & RoleBinding

Namespaces: Logical isolation for app resources

Secrets: Passwords and sensitive configs handled via Secret

🐳 Docker

A multi-stage Dockerfile is used to:

Install dependencies in a clean build layer

Copy only necessary artifacts to the final image

# Build stage
FROM node:18-alpine as builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .

# Production stage
FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app .
CMD ["node", "src/app.js"]

🔍 Trivy Security Scan

Trivy is used to scan the final Docker image:

trivy image yossefazozz/k8s_nodejs:latest

Make sure there are no CRITICAL vulnerabilities in production images.

⚙️ Technologies Used

Node.js

MongoDB 8 Replica Set

Kubernetes

Docker (multi-stage)

Trivy for image scanning

NGINX LoadBalancer

ConfigMap & Secrets

RBAC & Namespace Security

Persistent Volumes (PV, PVC)

Kubernetes Job for DB initialization

📦 Future Improvements

Add monitoring with Prometheus + Grafana

Add Ingress + TLS support

CI/CD pipeline with GitHub Actions or Jenkins

📚 Resources

Kubernetes Docs

MongoDB Replica Sets

Trivy

🧠 Author

Youssef Azozz

Feel free to open issues or contribute improvements!

📎 License

This project is licensed under the MIT License.

