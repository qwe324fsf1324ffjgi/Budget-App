Here's a `README.md` tailored for deploying a Ruby on Rails app on **Minikube (Windows)** or **Amazon EKS**, using the manifest files from the provided Google Drive link:

---

# 🚀 Ruby on Rails Kubernetes Deployment (Minikube & EKS)

This project provides a complete Kubernetes-based deployment pipeline for a Ruby on Rails application using **Minikube (Windows)** or **Amazon EKS**.

📁 **Manifests Location**
All required YAML files (Deployments, Services, PVCs, ArgoCD configs, etc.) are located here:
👉 [Google Drive - Kubernetes Manifests](https://drive.google.com/drive/folders/1X1fDOG-oGGdG5RaRbJ2AaCX9jBiNyNqy?usp=sharing)

---

## 🛠️ Prerequisites

Make sure the following tools are installed on your system:

### For Both Minikube & EKS:

* [Docker Desktop](https://www.docker.com/products/docker-desktop/)
* [kubectl](https://kubernetes.io/docs/tasks/tools/)
* [Git](https://git-scm.com/)
* [Helm](https://helm.sh/)
* [ArgoCD CLI](https://argo-cd.readthedocs.io/en/stable/cli_installation/) (Optional but recommended)

### Additional for Minikube:

* [Minikube](https://minikube.sigs.k8s.io/docs/start/)

### Additional for EKS:

* [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
* [eksctl](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html)

---

## 🚦 Getting Started

---

## ✅ Option 1: Deploy on Minikube (Windows)

### 1. Start Minikube and expose Docker daemon

```powershell
minikube start --driver=docker
```

Ensure Docker is linked:

```powershell
minikube docker-env
& minikube docker-env | Invoke-Expression
```

### 2. Clone the Repo & Download Manifest Files

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
```

> ⚠️ Download the manifests from the [Google Drive Link](https://drive.google.com/drive/folders/1X1fDOG-oGGdG5RaRbJ2AaCX9jBiNyNqy?usp=sharing) and place them in a `manifests/` folder inside your project.

### 3. Deploy Using `kubectl`

```bash
kubectl apply -f manifests/
```

This will create:

* PostgreSQL Deployment + Service
* Rails Deployment + Service
* Persistent Volume Claims
* ArgoCD application.yaml (if present)

---

## ✅ Option 2: Deploy on Amazon EKS

### 1. Create EKS Cluster

```bash
eksctl create cluster --name rails-cluster --region us-east-1 --nodes 2
```

### 2. Update Kubeconfig

```bash
aws eks --region us-east-1 update-kubeconfig --name rails-cluster
```

### 3. Deploy Your App

Clone the repo and apply the manifests:

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
kubectl apply -f manifests/
```

---

## 🚀 Optional: Use ArgoCD for GitOps

### 1. Install ArgoCD on Cluster

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### 2. Port Forward the ArgoCD UI

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Access ArgoCD at: [https://localhost:8080](https://localhost:8080)

### 3. Login to ArgoCD CLI

```bash
argocd login localhost:8080 --username admin --password <retrieved-password>
```

> To get the initial password:

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

### 4. Deploy Rails App via ArgoCD

Ensure `application.yaml` in your manifests folder is properly set to track your Git repo.

```bash
kubectl apply -f manifests/application.yaml -n argocd
```

---

## 🧪 Verifying the Deployment

* Check Pods:

```bash
kubectl get pods
```

* Check Services:

```bash
kubectl get svc
```

* Port-forward Rails Service (Minikube):

```bash
kubectl port-forward svc/rails-service 3000:3000
```

Open your browser at: [http://localhost:3000](http://localhost:3000)

---

## 🗃 Folder Structure Suggestion

```
project-root/
│
├── manifests/
│   ├── postgres.yaml
│   ├── rails.yaml
│   ├── pvc.yaml
│   ├── application.yaml
│   └── argocd-cm.yaml (optional)
│
└── README.md
```

---

## 🧹 Cleanup

### For Minikube:

```bash
minikube delete
```

### For EKS:

```bash
eksctl delete cluster --name rails-cluster --region us-east-1
```

---

## 🤝 Contributing

Feel free to fork and submit PRs or suggestions for improvement!

---

## 📄 License

MIT License

---

Let me know if you want this README adapted to a specific folder structure or want me to auto-generate a GitHub-ready version.
