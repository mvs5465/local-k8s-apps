# Local K8s Applications

ArgoCD Application definitions for the local K8s cluster. This repo is watched by ArgoCD's root applications.

## Structure

```
manifests/
├── kustomization.yaml     # Root kustomization that includes all apps
├── dashboard-ui/          # Dashboard UI app
│   ├── dashboard-ui-app.yaml
│   ├── dashboard-ui.yaml
│   └── kustomization.yaml
├── grafana/               # Grafana metrics UI
│   ├── grafana-app.yaml
│   └── kustomization.yaml
├── prometheus/            # Prometheus metrics
│   ├── prometheus-app.yaml
│   └── kustomization.yaml
└── fileserver/            # Static file server (optional)
    ├── fileserver-app.yaml
    ├── fileserver.yaml
    └── kustomization.yaml
```

## How It Works

- **Repo**: `local-k8s-argocd` contains ArgoCD infrastructure and root applications
- **This repo** (`local-k8s-apps`) contains application definitions organized by app
- Root application in `local-k8s-argocd` points to `manifests/`
- ArgoCD watches `main` branch and syncs all apps via Kustomize
- Each app is in its own subdirectory with its own `kustomization.yaml`
- Root `manifests/kustomization.yaml` includes all app subdirectories

## Enable/Disable Apps

Apps are enabled/disabled by modifying their `kustomization.yaml`:

**Fileserver (currently disabled by default):**
- Edit `manifests/fileserver/kustomization.yaml`
- Uncomment the `- fileserver.yaml` line in resources
- Ensure `/tmp/files` directory exists on k3s node: `colima ssh mkdir -p /tmp/files`

To disable other apps, remove or comment out their entries in `manifests/kustomization.yaml`

## Adding New Applications

1. Create new subdirectory: `manifests/<app-name>/`
2. Add application manifests (`<app-name>-app.yaml`, etc.)
3. Create `kustomization.yaml` listing the resources
4. Add reference to root `manifests/kustomization.yaml` under resources
5. Push to main branch

Example structure:
```yaml
# manifests/<app-name>/kustomization.yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
  - <app-name>-app.yaml
  - <app-name>.yaml  # if separate manifest

# manifests/kustomization.yaml (add your app)
resources:
  - <app-name>
```

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
