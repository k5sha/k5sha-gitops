<div align="center">

# ⚓ GitOps Infrastructure: ArgoCD & K3s
![K3s](https://img.shields.io/badge/K3s-%23FFC629.svg?style=for-the-badge&logo=k3s&logoColor=black)
![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)
![ArgoCD](https://img.shields.io/badge/ArgoCD-ef7b4d?style=for-the-badge&logo=argo&logoColor=white)
![Prometheus](https://img.shields.io/badge/Prometheus-E6522C?style=for-the-badge&logo=Prometheus&logoColor=white)
![Postgres](https://img.shields.io/badge/postgres-%23316192.svg?style=for-the-badge&logo=postgresql&logoColor=white)
</div>

## 🏗 Project Architecture

This repository serves as the **Single Source of Truth (SSOT)** for my lightweight Kubernetes cluster infrastructure. Hosted on a Virtual Private Server (VPS) running **K3s**, it strictly adheres to **GitOps** principles using **ArgoCD**, ensuring that the cluster state always matches the declarative configurations stored in this repository.


<img width="4450" height="auto" alt="K5sha-gitops"  src="https://github.com/user-attachments/assets/a9fab2a6-f9ec-4cbb-a36c-d02dd7f45940" />


[🔗 Open the scheme in the Lucidchart](https://lucid.app/lucidchart/6d25d56c-a659-48dd-a1b4-fc5e88a9af88/edit?viewport_loc=-158%2C-647%2C3037%2C1812%2C0_0&invitationId=inv_54d01f55-9acd-4caf-bc1a-574ca8b8f68d)
## 🧳 The App-of-Apps Pattern

To maintain scalability and clean logical separation, this repository implements the ArgoCD **App-of-Apps** pattern. 

Instead of deploying individual manifests manually, a single `root.yaml` application is deployed, which recursively discovers and synchronizes all other applications defined in the `apps/` directory.

### 📂 Repository Structure

```text
.
├── apps/               # ArgoCD Application Wrappers
│   ├── monitoring.yaml # Kube-Prometheus-Stack definition
│   ├── portfolio.yaml  # Portfolio app definition
│   └── tikceto.yaml    # Tikceto microservices definition
├── bootstrap/          # Cluster Initialization
│   └── root.yaml       # The "App of Apps" entry point
└── manifests/          # Raw Kubernetes Resources
    ├── portfolio/      # Deployment, Ingress, Service
    └── tikceto/        # Backend, Frontend, Minio, Postgres, Ingress
```
### ⚙️ Cluster Bootstrapping
To deploy this entire infrastructure to a fresh Kubernetes cluster (assuming ArgoCD is pre-installed):

**1. Apply the Root Application:**
```Bash
kubectl apply -f bootstrap/root.yaml
```
**2. Automated Synchronization:**

ArgoCD will detect the root application, which will subsequently deploy monitoring, portfolio, and tikceto. All dependencies, persistent volume claims, and ingresses will be provisioned automatically based on the sync policies.

### 📅 Backlog
- [ ] Add external secret manager
- [ ] Add the ELK Stack

---
<div align="center">

Made with ❤️ by Yurii Yevtushenko
</div>
