# 📦 ArgoCD Bootstrap

This folder contains the **ArgoCD bootstrap configuration** following the **App of Apps** pattern.

## 🔧 Purpose

The `bootstrap/` folder is used to declaratively tell ArgoCD **what applications to manage** in the cluster. It separates **cluster deployment logic** from individual app source code, enabling true GitOps.

## 🚀 How It Works

- `root-app.yaml` is applied manually once to ArgoCD.
- It registers all apps defined in the `app/` folder.
- ArgoCD then watches those apps and deploys them from their respective Git repos.

## 📘 Notes

- Keep this folder in a **separate Git repo** in production for centralized control.
- ArgoCD uses this to keep your cluster state in sync with Git — no manual changes needed!

---
