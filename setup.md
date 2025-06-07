# Installation and Cluster Setup

## Step 1: Install Helm

Run the following command to install Helm on your machine:

```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

Verify the installation:

```bash
helm version
```

---

## Step 2: Cluster Setup

Cluster Configuration: `kind-config.yaml`

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane
    extraPortMappings:
      - containerPort: 80 # for nginx ingress
        hostPort: 80
        protocol: TCP
      - containerPort: 443 # for HTTPS ingress
        hostPort: 443
        protocol: TCP
      - containerPort: 31000 # for frontend container
        hostPort: 31000
        protocol: TCP
      - containerPort: 31100 # for backend container
        hostPort: 31100
        protocol: TCP
      - containerPort: 30080 # for backend container
        hostPort: 30080
        protocol: TCP
    - containerPort: 30080 # for argocd ui
        hostPort: 30080
        protocol: TCP
```

- Run the following command to create a Kubernetes cluster using the provided `kind-config.yaml`:

  ```bash
  kind create cluster --config kind-config.yaml
  ```

- Create **mern-devops** namespace:

  ```bash
  kubectl create namespace mern-devops
  ```

---
