# GitOps Repository for IBM App Connect Integration Runtimes

This repository manages **IBM App Connect Integration Runtimes** using **Kustomize** and **ArgoCD** following the GitOps approach.

## Repository Structure
```
.
├── base
│   ├── kustomization.yaml
│   └── integrationruntime.yaml
│
├── envs
│   ├── dev
│   │   ├── .gitkeep
│   │   ├── tea-argo-cp4i
│   │   │   └── kustomization.yaml
│   │   └── configurationCRs
│   │       ├── kustomization.yaml
│   │       ├── default-policy.yaml
│   │       ├── teajdbc.yaml
│   │       └── teajdbc-policy.yaml
│   │
│   └── uat
│       ├── .gitkeep
│       ├── tea-argo-cp4i
│       │   └── kustomization.yaml
│       └── configurationCRs
│           ├── kustomization.yaml
│           ├── default-policy.yaml
│           ├── teajdbc.yaml
│           └── teajdbc-policy.yaml
```


## Overview

- `base/`  
  Contains the **common Integration Runtime configuration** shared by all environments.  
  - `integrationruntime.yaml` — Defines the CP4I Integration Runtime custom resource.  
  - `kustomization.yaml` — Kustomize entry point for the base.

- `envs/<environment>/`  
  Environment-specific overlays that extend/override the base configuration.  
  - `tea-argo-cp4i/` — Main Kustomize overlay for the environment.  
  - `configurationCRs/` — Additional configuration Custom Resources for the environment, such as policies and JDBC configuration.

## Flow Diagram

```mermaid
flowchart TD
    A[Base Config <br/> ./base] --> B[Environment Overlay <br/> ./envs/dev or ./envs/uat]
    B --> C[ArgoCD Application <br/> watches overlay path]
    C --> D[Cluster Namespace <br/> Integration Runtime Deployed]
