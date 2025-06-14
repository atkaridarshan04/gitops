# 🚀 GitOps Deployment with ArgoCD & Helm

This project demonstrates a **GitOps pipeline** using ArgoCD and Helm. It follows the **"App of Apps"** pattern to manage and deploy Kubernetes workloads declaratively via Git.


## 🧱 Project Structure

```
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
└── setup.md          # Install Helm and Setup k8s cluster
```

## 🎯 Key Concepts

| Component            | Role & Purpose                                                                                                                                                                                                                                                                |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **ArgoCD**           | A GitOps controller that continuously syncs your Kubernetes cluster with the desired state defined in Git. It ensures automation, visibility, and consistency across environments.                                                                                                                                        |
| **Bootstrap Folder** | Acts as the single source of truth for ArgoCD. It holds a root `Application` manifest that tells ArgoCD which other apps (Helm releases, K8s YAMLs, etc.) to manage. This enables scalable and modular management of multiple applications using the **App of Apps** pattern. |

> ⚙️ In essence, the `bootstrap/root-app.yaml` bootstraps the entire GitOps system by pointing ArgoCD to everything it should manage — including your actual app defined via Helm.


## 🚀 Setup Instructions

> Install Helm and Setup k8s cluster [setup.md](./setup.md)

### 1. Install ArgoCD

You can install ArgoCD locally using Helm:

```bash
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update

helm install argocd argo/argo-cd \
  -n argocd --create-namespace \
  -f argocd/helm-values.yaml
````

> ⚠️ Use `NodePort` service for local access (`helm-values.yaml` should reflect this)

### 2. Access ArgoCD UI

```bash
kubectl -n argocd get svc argocd-server
```

Open in browser at:
**[http://localhost:30080](http://localhost:30080)**

Get initial password:

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

### 3. Deploy the Root ArgoCD Application

This `root-app.yaml` will deploy all apps defined in the `bootstrap/apps/` folder.

```bash
kubectl apply -f bootstrap/root-app.yaml
```

> ArgoCD will now auto-sync and deploy your Helm-based app!

### 4. Verify the Deployments
Go to Argo Dashboard verify deployments
![argo-dash](./argo-dash.png)


Also check using 
```bash
kubectl get all -n mern-devops
```

## 🔄 Updating the App

* Commit changes to your Helm chart or `values.yaml`
* Push to Git
* ArgoCD will detect the change and **automatically sync**

You can also manually sync in the ArgoCD UI if automation is turned off.

---