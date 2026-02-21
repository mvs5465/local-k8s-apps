# Local K8s Applications

ArgoCD Application definitions for system and user-facing services. Pair with [`local-k8s-argocd`](https://github.com/mvs5465/local-k8s-argocd) infrastructure repo.

## Services Included

**System Tier** (monitoring & infrastructure):
- **Prometheus**: Metrics collection from all pods
- **Grafana**: Dashboards (Cluster Overview, Loki Logs)
- **Loki**: Log aggregation backend
- **Promtail**: Log collection agent (runs on all nodes)
- **Nginx Ingress**: Routes external traffic to services

**Services Tier** (user-facing):
- **Homepage**: Service dashboard with live k8s cluster widget
- **Gatus**: Uptime monitoring and status page
- **Jellyfin**: Media server

## Structure

```
apps/
├── system-app.yaml         # Parent for system services
├── services-app.yaml       # Parent for user services
├── system/
│   ├── prometheus-app.yaml
│   ├── grafana-app.yaml
│   ├── loki-app.yaml
│   ├── promtail-app.yaml
│   └── nginx-ingress-app.yaml
└── services/
    ├── homepage-app.yaml
    ├── gatus-app.yaml
    └── jellyfin-app.yaml

manifests/
├── prometheus/
├── grafana/
├── loki/
├── promtail/
├── nginx-ingress/
├── homepage/
├── gatus/
└── jellyfin/
```

## Adding Apps

1. Create `<app-name>-app.yaml` in `apps/system/` or `apps/services/`
2. Reference your Helm chart or git repo
3. Push to main—ArgoCD auto-discovers it

## Development

Create feature branches to test changes. ArgoCD watches main, so changes sync automatically after merge.

See `CLAUDE.md` for development guidelines.
