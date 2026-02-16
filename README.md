# Local K8s Applications

ArgoCD Application definitions for the local K8s cluster. This repo is watched by ArgoCD's root applications.

## Structure

```
system/
├── grafana-app.yaml       # System monitoring UI
└── prometheus-app.yaml    # System metrics collection

apps/
├── dashboard-ui-app.yaml  # Local cluster dashboard
└── fileserver-app.yaml    # Static file server
```

## How It Works

- **Repo**: `local-k8s-argocd` contains ArgoCD infrastructure and root applications
- **This repo** (`local-k8s-apps`) contains Application definitions
- Root applications in `local-k8s-argocd` point here and auto-discover applications
- ArgoCD watches `main` branch in this repo

## Adding New Applications

1. Create `<app-name>-app.yaml` in either `system/` or `apps/`
2. Push to main branch
3. ArgoCD auto-discovers and deploys

Example structure:
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-app
spec:
  project: cluster
  source:
    repoURL: <your-app-repo>
    targetRevision: main
    path: manifests
  destination:
    server: https://kubernetes.default.svc
    namespace: default
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

## Enabling Optional Applications

**Fileserver** (currently disabled):
- File: `manifests/fileserver-app.yaml.disabled`
- To enable: Rename to `manifests/fileserver-app.yaml`
- Requirements: `/tmp/files` directory must exist on k3s node
- Setup: `colima ssh mkdir -p /tmp/files`

## Two-Repo Architecture

This setup prevents the "chicken-and-egg" problem:

- **local-k8s-argocd**: Infrastructure repo (stable, ArgoCD config)
  - Rarely changes
  - Contains ArgoCD installation and root apps
  - Always on main branch

- **local-k8s-apps**: Applications repo (active development)
  - Where you add/modify/test applications
  - Can iterate on feature branches safely
  - ArgoCD auto-discovers from main

Benefits:
- Test application changes without touching ArgoCD
- Clear separation of concerns
- Safer production deployments (config changes less frequent)
