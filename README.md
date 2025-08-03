<h1 align="center">🚀 Kubernetes-Based Node.js Microservice with MongoDB</h1>

This project demonstrates how to deploy a **production-ready Node.js microservice** with a **MongoDB replica set** on **Kubernetes**, implementing **best practices for containerization, security, database clustering, and infrastructure automation**.
---

## 📖 Overview
This project demonstrates how to deploy a **scalable, secure, and production-ready Node.js microservice** 
with a **MongoDB replica set** on **Kubernetes**, following **best practices for:**

- Containerization
- Security & Secrets Management
- Stateful & Stateless Workloads
- Database Clustering

<h2>🧩 Architecture</h2>

```plaintext
Client → NGINX LoadBalancer → Node.js Pods → MongoDB Replica Set
Key Components:
1) NGINX LoadBalancer handles incoming traffic.
2) Node.js Application Pods scale horizontally with a Deployment.
3) MongoDB Replica Set (StatefulSet) ensures data consistency.
4) Headless Service allows internal communication between MongoDB replicas.
5) Persistent Volumes keep MongoDB data safe.
6) Kubernetes Job initializes the replica set and MongoDB user.
7) Namespace + RBAC provide workload isolation and security.

<p align="center"> <img src="images/k8s-architecture.png" alt="Kubernetes Architecture" width="650"/> </p>



📂 Project Structure

final_k8s_project/
├── k8s/                          # Kubernetes manifests
│   ├── namespace.yaml            # Namespace definition
│   ├── configmap.yaml            # App config via ConfigMap
│   ├── secrets.yaml              # Sensitive data via Secret
│   ├── node-app-deployment.yaml  # Node.js app Deployment
│   ├── mongo-statefulset.yaml    # MongoDB replica set StatefulSet
│   ├── mongo-service.yaml        # MongoDB headless service
│   ├── mongo-init-job.yaml       # Job to init replica set & create user
│   └── rbac.yaml                 # RBAC roles and bindings
├── src/                          # Node.js application source code
│   └── ...                       # JavaScript logic
├── Dockerfile                    # Multi-stage Docker build
├── .dockerignore                 # Files to ignore in Docker build
├── .trivyignore                  # Trivy security scan ignore rules
├── package.json                  # Node.js dependencies and scripts
└── README.md                     # Project documentation


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

