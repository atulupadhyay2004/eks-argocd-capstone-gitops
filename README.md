# 🚀 EKS GitOps Capstone with ArgoCD

[![GitHub repo size](https://img.shields.io/github/repo-size/atulupadhyay2004/eks-argocd-capstone-gitops)](https://github.com/atulupadhyay2004/eks-argocd-capstone-gitops)
[![GitHub stars](https://img.shields.io/github/stars/atulupadhyay2004/eks-argocd-capstone-gitops?style=social)](https://github.com/atulupadhyay2004/eks-argocd-capstone-gitops/stargazers)

> A production-grade GitOps repository demonstrating automated deployments on **Amazon EKS** using **ArgoCD**, featuring **sync waves**, **post-sync smoke tests**, and **rolling updates** with zero downtime.

---

## 📋 Table of Contents
- [About The Project](#-about-the-project)
- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [System Architecture](#-system-architecture)
- [GitOps Workflow](#-gitops-workflow)
- [ArgoCD Sync Waves](#-argocd-sync-waves)
- [Helm Chart Structure](#-helm-chart-structure)
- [Smoke Testing Strategy](#-smoke-testing-strategy)
- [Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Deploy with ArgoCD](#deploy-with-argocd)
  - [Trigger Auto-Sync](#trigger-auto-sync)
- [Contributing](#-contributing)
- [License](#-license)
- [Contact](#-contact)

---

## 📖 About The Project

This repository serves as the **single source of truth** for a Node.js application deployed on **Amazon EKS**. It implements a complete GitOps workflow where:

- The **Helm chart** defines the application's desired state
- **ArgoCD** continuously monitors the repository and syncs the cluster
- **Sync waves** ensure resources are created in the correct order
- **Post-sync smoke tests** validate the deployment before marking it as successful

The application is a production-ready Node.js service with health checks, resource limits, and zero-downtime rolling updates.

---

## ✨ Features

- ✅ **GitOps-driven** — Declarative infrastructure as code
- ✅ **Amazon EKS** — Production-grade Kubernetes on AWS
- ✅ **ArgoCD sync waves** — Ordered resource deployment (ConfigMap → Deployment → Smoke Test)
- ✅ **Automated smoke tests** — Post-sync validation of critical endpoints
- ✅ **Zero-downtime rolling updates** — RollingUpdate strategy with maxSurge and maxUnavailable
- ✅ **Health checks** — Liveness and readiness probes for application stability
- ✅ **Resource management** — CPU and memory requests/limits defined
- ✅ **ConfigMap injection** — Environment variables managed via ConfigMap

---

## 🛠 Tech Stack

| Category | Technology |
|----------|------------|
| **Cloud Provider** | Amazon Web Services (EKS) |
| **Orchestration** | Kubernetes (EKS) |
| **GitOps Operator** | ArgoCD |
| **Package Manager** | Helm (v3) |
| **Application** | Node.js (atulupadhyay002/eks-argocd-capstone) |
| **Registry** | Docker Hub |
| **Testing** | Curl-based smoke tests (PostSync hook) |
| **Version Control** | Git / GitHub |

---


---

## 🔄 GitOps Workflow

This diagram illustrates the end-to-end GitOps flow with sync waves:

```mermaid
flowchart TD
    A[Developer updates Helm chart or values] --> B[git add, commit, push]
    B --> C[GitHub receives the change]
    C --> D[ArgoCD polls GitHub every 3 minutes]
    D --> E{Detected change?}
    E -->|No| D
    E -->|Yes| F[ArgoCD identifies drift]
    F --> G[Sync Wave 0: Apply ConfigMap]
    G --> H[Sync Wave 1: Apply Deployment & Service]
    H --> I[RollingUpdate creates new pods]
    I --> J[Readiness probes verify new pods are ready]
    J --> K{All pods ready?}
    K -->|No| L[Rollback - keep old pods]
    K -->|Yes| M[Old pods terminated]
    M --> N[Sync Wave 2: Execute PostSync Smoke Test]
    N --> O[Smoke test runs curl checks]
    O --> P{All endpoints pass?}
    P -->|No| Q[Sync marked as Failed - Alert]
    P -->|Yes| R[Smoke test job auto-deletes]
    R --> S[Sync marked as Successful]
    S --> T[Application is live on EKS]
