# GitOps Repository for IBM App Connect Integration Runtimes

This repository manages **IBM App Connect Integration Runtimes** using **Kustomize** and **ArgoCD** following the GitOps approach.

## Overview

This repository leverages Kustomize's **base** and **overlay** concepts to manage IBM App Connect Integration Runtime deployments across different environments in a scalable and maintainable way.

### Base

The `base/` directory holds the core, reusable configuration for the Integration Runtime. This includes:

- `integrationruntime.yaml`: The main custom resource definition for the Integration Runtime.
- `kustomization.yaml`: The entry point for Kustomize.

The base is designed to be environment-agnostic, containing only the common settings and resources that apply to all environments. No environment-specific values (such as secrets, resource limits, or policies) should be placed here.

### Overlays

The `envs/` directory contains overlays for each environment (e.g., `dev`, `uat`). Each overlay references the base and customizes it for its environment. Overlays allow you to:

- Override or extend base configuration (such as resource limits, environment variables, or secrets).
- Add environment-specific custom resources (like policies or JDBC configurations).
- Manage differences between environments (development, testing, production) while reusing the shared base.

**How it works:**  
When deploying, Kustomize combines the base with the selected overlay, producing a complete manifest tailored for the target environment. This ensures consistency across environments while allowing necessary customization.

### Repository Structure

```bash
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

## Image Registry Access

To allow environments to pull images from the registry in a different namespace (e.g. `cp4i`), run:

```bash
oc -n cp4i policy add-role-to-user system:image-puller system:serviceaccount:dev:default
oc -n cp4i policy add-role-to-user system:image-puller system:serviceaccount:uat:default
```

## Usage

1. **Clone this repository** to your local machine.
2. **Customize overlays** in `envs/dev` or `envs/uat` as needed.
3. **Apply with ArgoCD** or manually using `kubectl`/`oc` and Kustomize.
