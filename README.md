
---

# 🚀 Ruby on Rails Kubernetes Deployment (Minikube & EKS)

This project provides a complete Kubernetes-based deployment pipeline for a Ruby on Rails application using **Minikube (Windows)** or **Amazon EKS**.

📁 **Manifests Location**
All required YAML files (Deployments, Services, PVCs, ArgoCD configs, etc.) are located here:
👉 [Google Drive - Kubernetes Manifests](https://drive.google.com/drive/folders/1X1fDOG-oGGdG5RaRbJ2AaCX9jBiNyNqy?usp=sharing)

🎥 **Demo Video (Proof of Successful Deployment)**
See the live demo of the Rails application running successfully on Kubernetes:
👉 [Deployment Demo - Google Drive](https://drive.google.com/file/d/1IwreCdwLHXYjD-VQnB6wTkh09GXmjJB-/view?usp=sharing)

---

## 🧩 How I Achieved This on Windows (Minikube)

Here are the steps I followed to successfully deploy the Rails + PostgreSQL app on Minikube (Windows):

### 1. ✅ Installed Required Tools

I installed the following on Windows:

* [Minikube](https://minikube.sigs.k8s.io/docs/start/)
* [Docker Desktop](https://www.docker.com/products/docker-desktop/) (enabled Kubernetes integration)
* [kubectl CLI](https://kubernetes.io/docs/tasks/tools/)
* [Helm](https://helm.sh/)
* [Git](https://git-scm.com/)
* [ArgoCD CLI](https://argo-cd.readthedocs.io/en/stable/cli_installation/) (optional)

---

### 2. 🚀 Started Minikube

```powershell
minikube start --driver=docker
```

---

### 3. 🔁 Connected Docker to Minikube

```powershell
& minikube docker-env | Invoke-Expression
```

This allowed Docker builds to be available inside the Minikube cluster.

---

### 4. 🧱 Built the Rails App Image (Optional if using prebuilt image)

If I had made changes, I built the image locally with Docker:

```powershell
docker build -t budget-app:latest .
```

Then I confirmed it’s available inside Minikube using:

```bash
minikube image load budget-app:latest
```

---

### 5. 📥 Downloaded & Applied Manifests

I downloaded the manifests from Google Drive and stored them in a local `manifests/` folder, then applied them:

```bash
kubectl apply -f manifests/
```

This included:

* `rails.yaml`
* `postgres.yaml`
* PVCs
* `application.yaml` (for ArgoCD GitOps)

---

### 6. 🎯 Verified Deployment

```bash
kubectl get pods
kubectl get svc
```

I used port-forwarding to test the app locally:

```bash
kubectl port-forward svc/rails-service 3000:3000
```

Then opened:
🔗 [http://localhost:3000](http://localhost:3000)

---

### 7. ✅ Confirmed Everything Worked

Captured and shared this deployment proof:
👉 [Demo Video](https://drive.google.com/file/d/1IwreCdwLHXYjD-VQnB6wTkh09GXmjJB-/view?usp=sharing)

---

