# Local K8s Applications

ArgoCD Application definitions for the local K8s cluster.

## Repository Structure

```
apps/
├── system-app.yaml          # Parent app for system services
├── services-app.yaml        # Parent app for application services
├── system/
│   ├── prometheus-app.yaml  # Prometheus metrics collection
│   └── grafana-app.yaml     # Grafana visualization UI
└── services/
    ├── dashboard-ui-app.yaml      # Cluster dashboard
    └── fileserver-app.yaml.disabled  # Static file server (disabled by default)

manifests/
├── prometheus/              # Prometheus manifest files
├── grafana/                 # Grafana manifest files
├── dashboard-ui/            # Dashboard UI manifest files
└── fileserver/              # Fileserver manifest files
```

## How It Works

- **local-k8s-argocd** points root app to `apps/` directory
- Parent apps (`system-app.yaml`, `services-app.yaml`) in `apps/` point to their respective subdirectories
- Each parent app auto-discovers child applications in its subdirectory
- Actual manifests are in `manifests/` with one directory per app
- Each child Application file specifies its path in `manifests/`

## Enabling Optional Applications

**Fileserver** (currently disabled):
- File: `apps/services/fileserver-app.yaml.disabled`
- To enable: Rename to `fileserver-app.yaml`
- Requirements: `/tmp/files` directory must exist on k3s node
- Setup: `colima ssh mkdir -p /tmp/files`
- Then push to main—ArgoCD will auto-discover and deploy

## Adding New Applications

1. **Add manifest:** Create directory in `manifests/<app-name>/` with manifest files
2. **Add ArgoCD app:** Create `<app-name>-app.yaml` in `apps/system/` or `apps/services/`
3. **Point to manifests:** Set `path: manifests/<app-name>` in the Application spec
4. **Push to main:** Root app will auto-discover

## Two-Repo Architecture

This setup prevents the "chicken-and-egg" problem:

- **local-k8s-argocd**: Infrastructure repo (stable, ArgoCD config)
  - Rarely changes
  - Contains ArgoCD installation and root apps
  - Points to `local-k8s-apps/apps/`

- **local-k8s-apps**: Applications repo (active development)
  - Where you add/modify/test applications
  - Can iterate on feature branches safely
  - ArgoCD auto-discovers from main

Benefits:
- Test application changes without touching ArgoCD
- Clear separation of concerns
- Safer production deployments (config changes less frequent)
