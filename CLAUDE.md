# Local K8s Applications - Claude Guidelines

This repo contains ArgoCD Application definitions. Pair with `local-k8s-argocd` infrastructure repo.

## Key Points

- **Watched by**: ArgoCD root-system and root-apps applications
- **Update frequency**: As needed for new/modified applications
- **Structure**: `system/` and `apps/` directories by tier
- **Commit style**: Same Conventional Commits as main repo

## Common Tasks

### Add new application
1. Create `<name>-app.yaml` in `system/` or `apps/`
2. Reference your application git repo
3. Push to main → ArgoCD auto-discovers

### Test application changes
1. Create feature branch
2. Modify Application manifests
3. Test locally or in dev environment
4. Merge to main when ready

### Application template
See README.md for example Application structure.

## Integration with local-k8s-argocd

Root applications in `local-k8s-argocd` point here:
- `root-system-app.yaml` → watches `system/` directory
- `root-apps-app.yaml` → watches `apps/` directory

Do NOT manually apply applications - they're auto-discovered by root apps.
