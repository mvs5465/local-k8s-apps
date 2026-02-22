# Release Notes

## [Unreleased]

## [v1.2.0] - 2026-02-21 - Outline Wiki Stable Release

### Added
- **Outline Wiki** — Stable personal wiki with real-time collaboration support
  - PostgreSQL persistence via hostPath volumes in ~/outline/postgres
  - Redis for real-time collaboration sessions and performance
  - Integrated into Gatus uptime monitoring dashboard
  - Added to Homepage service discovery widget
  - Data persists through Colima VM restarts

### Fixed
- Outline PostgreSQL hostPath permissions via fsGroup (500)
- Outline memory tuning with NODE_OPTIONS for stable startup
- Outline FORCE_HTTPS disabled for Kubernetes health checks
- Outline service networking with headless PostgreSQL and Redis services
- Homepage bookmarks configuration structure per spec
- Grafana dashboard auto-star on load

### Changed
- Outline now documented as stable Services Tier application
- Removed External Secrets Operator (no longer needed after fixes)
- Homepage bookmarks restructured with GitHub repo links
- Updated all service manifests to documentation

## [v1.0.0] - 2026-02-20 - Phase 1 Complete: Custom Dashboards & Monitoring

### Added
- **Cluster Overview Dashboard** — custom 6-panel Grafana dashboard with verified PromQL queries:
  - Node CPU/memory utilization (stat panels with thresholds)
  - Pod CPU/memory usage (timeseries by pod + namespace)
  - Pod restart counts (1h window, bar chart)
  - Node network I/O (RX/TX on same panel, bytes/sec)
  - Replaces broken gnetId:12740 with working, maintainable alternative
- **Loki Logs Dashboard** — custom log browser dashboard:
  - Namespace + Pod dropdowns (populated from Loki labels)
  - Log volume graph (stacked bars by namespace)
  - Log stream viewer with expandable details
  - Log level filter dropdown: error|warn (default), error, warn, info, debug, all
  - Uses correct LogQL operators (`|~` for line filtering)
  - Replaces broken gnetId:13186 (2020, incompatible with Loki 3.x strict matchers)
- Dashboards stored as inline JSON in Helm values — GitOps-managed by ArgoCD, survive pod restarts

### Changed
- Grafana dashboards now custom (stored in `grafana-app.yaml` values)
- Removed imported gnetId dashboards (gnetId:12740, gnetId:13186)

### Fixed
- Pod-level CPU metrics: use `container_cpu_usage_seconds_total{cpu="total"}` (cadvisor no longer exposes per-container CPU)
- Loki dashboard template variables: `query` field must be comma-separated string (not empty) for Grafana to render options
- Loki line filtering: replaced `| regexp` with `|~` (correct LogQL operator for content filtering)
- Loki matcher patterns: use `.+` instead of `.*` to avoid "empty-compatible" matcher rejection in Loki 3.x

### Dashboards Details
All dashboard JSON stored in `apps/system/grafana-app.yaml` under `dashboards.default`:
- **cluster-overview** — custom inline JSON
- **loki-logs** — custom inline JSON

## [v0.1.0] - Initial Release

Initial applications deployment structure with Prometheus, Grafana, Loki, Promtail, Homepage, Gatus.
