# 🌐 Anicord Webserver Deployment

This directory contains the Kubernetes manifests for the Anicord Webserver and its MariaDB backend.

## 🏗️ Architecture

The application is composed of two main components:
- **Node.js Webserver**: The core application logic.
- **MariaDB**: The persistent database backend.
- **Redis**: In-memory data structure store used as a cache between the webserver and database.

### Key Features
- **Security**: Runs as non-root (UID 1000) and enforced by Kubernetes Pod Security baseline.
- **Networking**: Protected by **Cilium Network Policies** and exposed via a **LoadBalancer** service on NodePort `13000`.
- **Persistence**: Database state is preserved using **Longhorn** storage (4Gi).
- **Secret Management**: Integrated with **SOPS** for git-safe secret storage.

## 🚀 Deployment Guide

### 1. Configure Secrets
Edit the `secrets.yaml` file with your production credentials.
> [!IMPORTANT]
> Never commit unencrypted secrets to Git!

### 2. Encrypt with SOPS
Once configured, encrypt the secrets file using your SOPS key:
```bash
sops --encrypt --in-place webserver/secrets.yaml
```

### 3. Deploy
Apply the configuration using Kustomize, and apply the secrets separately using SOPS:
```bash
# 1. Apply the main infrastructure
kubectl apply -k webserver/

# 2. Decrypt and apply the secrets
sops -d webserver/secrets.yaml | kubectl apply -f -
```

## 🛠️ Components Detail

| Component | Image | Port | Storage |
| :--- | :--- | :--- | :--- |
| **Webserver** | `ghcr.io/anicord-acow/webserver` | `80` (NodePort `13000`) | N/A |
| **MariaDB** | `mariadb` | `3306` | `4Gi` (Longhorn) |
| **Redis** | `redis:7-alpine` | `6379` | `1Gi` (Longhorn) |

## 🛡️ Network Policy
The deployment includes a `CiliumNetworkPolicy` that restricts traffic to ensure safe communication between the webserver and its database.
