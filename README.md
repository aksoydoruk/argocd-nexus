# ArgoCD-Managed Nexus Deployment for OpenShift  
*A complete GitOps solution for Nexus Repository Manager, Docker Registry Hosting, and Air-Gap Image Workflows*

---

## 📘 Overview

This repository provides a **production-ready GitOps implementation** of **Sonatype Nexus Repository Manager** on **OpenShift**, fully orchestrated by **Argo CD**.  
It includes:

- Deployment of the Nexus operator via OLM  
- Automated provisioning of a NexusRepo instance  
- Exposure of Nexus UI via OpenShift Routes  
- Exposure of a Nexus-backed Docker Registry on port `5000`  
- Ideal setup for **air-gapped installations**, **CP4D image mirroring**, **internal CI/CD pipelines**, and **enterprise GitOps workflows**

This project follows strict GitOps principles:
- **Declarative Infrastructure**
- **Version-Controlled State**
- **Automated Deployment & Reconciliation**
- **Self-Healing Application State**

---

## 🏗 Repository Structure

argocd-nexus/
├── 10-nexus/ # All Nexus Kubernetes manifests
│ ├── nexus-namespace.yaml
│ ├── nexus-operator-subscription.yaml
│ ├── nexus-instance.yaml
│ ├── nexus-ui-route.yaml
│ ├── nexus-docker-service.yaml
│ └── nexus-docker-route.yaml
│
└── 90-argocd-apps/ # Argo CD Application definitions
└── nexus-app.yaml

yaml
Copy code

---

# 🔄 Architecture Diagram (GitOps Flow)

┌──────────────────┐ Git Pull ┌────────────────────┐
│ Git Repository │ --------------------> │ Argo CD │
│ (Desired State) │ │ (Reconciler Loop) │
└──────────────────┘ └─────────┬──────────┘
│ Sync
▼
┌────────────────────┐
│ OpenShift Cluster │
│ - Nexus Operator │
│ - Nexus Instance │
│ - Routes / SVCs │
└────────────────────┘

yaml
Copy code

---

# 🚀 Deployment Instructions

## 1. Clone the repository

bash
git clone https://github.com/aksoydoruk/argocd-nexus.git
cd argocd-nexus
2. Create the Nexus namespace
bash
Copy code
oc apply -f 10-nexus/nexus-namespace.yaml
3. Deploy the Argo CD application
bash
Copy code
oc apply -f 90-argocd-apps/nexus-app.yaml
4. Synchronize from Argo CD UI
Open ArgoCD → Applications → nexus

Press SYNC

Nexus Operator + Repository + Routes are created automatically

📦 Components Included
🧩 Nexus Operator Subscription
Installs the official Nexus Repository Operator from OLM catalogs.

🧩 NexusRepo Instance
Defines the Nexus deployment:

replica count

storage configuration

resource limits

admin credentials

persistence settings

Update the spec: section to match your environment by running:

bash
Copy code
oc get nexusrepo -n nexus -o yaml
🧩 Nexus UI Route
Exposes the management UI via:

pgsql
Copy code
https://nexus.apps.<cluster-domain>/
🧩 Docker Registry Service & Route
Enables usage of Nexus as an internal Docker Registry:

Example:

ruby
Copy code
docker login nexus-registry.<domain>
docker push nexus-registry.<domain>/repo/image:tag
docker pull nexus-registry.<domain>/repo/image:tag
Used for:

Air-gapped OpenShift & Cloud Pak installations

CP4D image mirroring

Internal DevSecOps pipelines

Private enterprise registries

🛡 GitOps Advantages
Feature	Benefit
Self-Healing	Manual changes are automatically corrected
Declarative State	Cluster configuration stored in Git
Multi-Cluster Portability	Same manifests work across environments
Version Control	Every change tracked & auditable
Rapid Recovery	Rollbacks via git revert
Consistency	Identical Nexus setup across all clusters

🔧 Air-Gap Scenarios (Cloud Pak for Data Example)
This GitOps-managed Nexus registry is ideal for offline installations.

Example workflow:
Deploy Nexus using this repo

Log into Nexus registry:

bash
Copy code
podman login nexus-registry.<domain> -u admin -p <password>
Mirror CP4D images:

bash
Copy code
cpd-cli mirror images \
  --registry=nexus-registry.<domain>/cpd \
  --user=admin \
  --password=<pw>
Install CP4D using the mirrored registry:

bash
Copy code
cpd-cli install <component> \
  --private-registry-location=nexus-registry.<domain>/cpd
🎛 Recommended Enhancements
Use Kustomize + values.env for cluster domain templating

Add SealedSecrets for registry credentials

Configure Resource limits & autoscaling

Integrate with Tekton pipelines

Add image-mirroring jobs to GitOps workflow

Enable TLS passthrough / re-encrypt modes

Configure backup strategies for Nexus data

📜 Requirements
OpenShift 4.x

Argo CD or OpenShift GitOps Operator

Nexus Operator installed via OLM

Valid persistent storage (OCS/ODF/Rook/Ceph)

🧪 Testing
Test Nexus UI Route:
bash
Copy code
curl -vk https://nexus.apps.<cluster-domain>
Test Docker Registry:
bash
Copy code
podman login nexus-registry.apps.<cluster-domain> -u admin -p admin
podman push nexus-registry.apps.<cluster-domain>/test/image:1
podman pull nexus-registry.apps.<cluster-domain>/test/image:1
🤝 Contributing
Contributions and PRs are welcome.
This repo is intended to be a reusable GitOps module for OpenShift environments.

📝 License
This repository is provided for enterprise automation use cases and may be freely adapted.

yaml
Copy code

---
