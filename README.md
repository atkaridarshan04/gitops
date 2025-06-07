# 🚀 GitOps Deployment with ArgoCD & Helm

This project demonstrates a **production-grade GitOps pipeline** using ArgoCD and Helm. It follows the **"App of Apps"** pattern to manage and deploy Kubernetes workloads declaratively via Git.


## 🧱 Project Structure

```graphql
.
├── argocd
│   └── helm-values.yml
├── bootstrap
│   ├── app
│   │   └── app.yml
│   └── root-app.yml
├── helm-chart
│   ├── Chart.yaml
│   ├── templates
│   └── values.yaml
├── README.md
└── setup.md
```

## 🎯 What We're Doing (and Why)

| Component      | Purpose                                                                 |
|----------------|-------------------------------------------------------------------------|
| **Helm Chart** | Packages your app for reusable, parameterized Kubernetes deployment     |
| **ArgoCD**     | GitOps controller that keeps your cluster in sync with your Git repo    |
| **Bootstrap**  | Central repo/folder that defines _what_ ArgoCD should manage and deploy |


## ✅ What This Setup Enables

- 🔄 **Continuous Delivery**: Git push = automatic app deployment
- 🧪 **Drift Detection**: ArgoCD alerts or fixes changes made outside Git
- 🔒 **Secure & Auditable**: Git becomes the source of truth
- ☸️ **Environment Ready**: Easily scale to dev/staging/prod clusters
- 📦 **Reusable Helm Chart**: Clean separation of code and config


## 🚀 Setup Instructions

> Install Helm and Setup k8s cluster [setup.md](./setup.md)

### 1. Install ArgoCD

You can install ArgoCD locally using Helm:

```bash
helm repo add argo https://argoproj.github.io/argo-helm
helm install argocd argo/argo-cd -n argocd --create-namespace -f helm-values.yaml
````

> ⚠️ Use `NodePort` service for local access (`helm-values.yaml` should reflect this)

### 2. Access ArgoCD UI

```bash
kubectl -n argocd get svc argocd-server
```

Open in browser at:
**[http://localhost](http://localhost):30080**

Get initial password:

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

---

### 3. Deploy the Root ArgoCD Application

This `root-app.yaml` will deploy all apps defined in the `bootstrap/apps/` folder.

```bash
kubectl apply -f bootstrap/root-app.yaml
```

> ArgoCD will now auto-sync and deploy your Helm-based app!


## 🔄 Updating the App

* Commit changes to your Helm chart or `values.yaml`
* Push to Git
* ArgoCD will detect the change and **automatically sync**

You can also manually sync in the ArgoCD UI if automation is turned off.

---