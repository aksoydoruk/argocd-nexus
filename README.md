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

