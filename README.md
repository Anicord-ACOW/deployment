# 🚀 Anicord Infrastructure

Welcome to the Anicord deployment repository. This project manages the Kubernetes manifests for the Anicord ecosystem using Kustomize and SOPS.

## 📂 Project Structure

- **[`webserver/`](./webserver)**: Main API and Frontend webserver with MariaDB backend.
- **`discord-bot/`**: (Planned) Configuration for the Anicord Discord integration.

## 🛠️ Global Requirements

- **Kubernetes Cluster**: K3s (preferred)
- **Storage**: Longhorn
- **Networking**: Cilium
- **Encryption**: SOPS + GPG/Age

## 🚦 Quick Start

1. **Clone the repo**:
   ```bash
   git clone https://github.com/Anicord-ACOW/anicord-deployment.git
   cd anicord-deployment
   ```

2. **Initialize Secrets**:
   Follow the instructions in the respective application directories to configure and encrypt your `secrets.yaml`.

3. **Deploy All**:
   ```bash
   kubectl apply -k .
   ```

---
*Maintained by the Anicord Team.*
