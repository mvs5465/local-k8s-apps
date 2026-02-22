# Local K8s Applications

ArgoCD Application definitions for system and user-facing services. Pair with [`local-k8s-argocd`](https://github.com/mvs5465/local-k8s-argocd) infrastructure repo.

## Apps Included

### System Services
| | | |
|---|---|---|
| 📊 **Prometheus** | Metrics collection and storage | [prometheus-app.yaml](apps/system/prometheus-app.yaml) |
| 📈 **Grafana** | Dashboards & visualization | [grafana-app.yaml](apps/system/grafana-app.yaml) |
| 📝 **Loki** | Log aggregation backend | [loki-app.yaml](apps/system/loki-app.yaml) |
| 🔍 **Promtail** | Log collection agent | [promtail-app.yaml](apps/system/promtail-app.yaml) |
| 🔔 **Prometheus Operator** | Kubernetes native monitoring | [prometheus-operator-app.yaml](apps/system/prometheus-operator-app.yaml) |
| 🌐 **Nginx Ingress** | External traffic routing | [nginx-ingress-app.yaml](apps/system/nginx-ingress-app.yaml) |

### User-Facing Services
| | | |
|---|---|---|
| 🏠 **Homepage** | Service dashboard with live k8s widget | [homepage-app.yaml](apps/services/homepage-app.yaml) |
| 📊 **Gatus** | Uptime monitoring & status page | [gatus-app.yaml](apps/services/gatus-app.yaml) |
| 🎬 **Jellyfin** | Media server | [jellyfin-app.yaml](apps/services/jellyfin-app.yaml) |
| 📖 **Outline** | Personal wiki with real-time collaboration | [outline-app.yaml](apps/services/outline-app.yaml) |
| 💬 **Open WebUI Chat** | Chat interface for Ollama | [chat-app.yaml](apps/services/chat-app.yaml) |
| 🤖 **Ollama** | LLM inference server | [ollama-app.yaml](apps/services/ollama-app.yaml) |

## Adding Apps

1. Create `<app-name>-app.yaml` in `apps/system/` or `apps/services/`
2. Reference your Helm chart or git repo
3. Push to main—ArgoCD auto-discovers it

## Development

Create feature branches to test changes. ArgoCD watches main, so changes sync automatically after merge.

See `CLAUDE.md` for development guidelines.
