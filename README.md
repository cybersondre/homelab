# 🏠 Homelab

A self-hosted Kubernetes homelab running on bare metal, managed with GitOps via ArgoCD.

## Hardware

| Node | Role | CPU | RAM | Storage |
|------|------|-----|-----|---------|
| node01 | Control Plane | Intel i3 | 16GB | 128GB |
| node02 | Worker | Intel i5 | 16GB | 128GB |
| node03 | Worker | Intel i5 | 16GB | 128GB |

All nodes are Lenovo ThinkCentre machines running Ubuntu Server 24.04 LTS.

## Stack

| Tool | Purpose |
|------|---------|
| [K3s](https://k3s.io) | Lightweight Kubernetes distribution |
| [ArgoCD](https://argo-cd.readthedocs.io) | GitOps continuous delivery |
| [Traefik](https://traefik.io) | Ingress controller (bundled with K3s) |
| [Longhorn](https://longhorn.io) | Distributed persistent storage |
| [Prometheus](https://prometheus.io) | Metrics collection |
| [Grafana](https://grafana.com) | Metrics visualization and dashboards |
| [Tailscale](https://tailscale.com) | Secure remote access |

## Architecture

```
GitHub (homelab repo)
        │
        │  GitOps sync
        ▼
     ArgoCD
        │
        │  deploys
        ▼
  K3s Cluster
  ┌─────────────────────────────────┐
  │  node01 (control plane)         │
  │  node02 (worker)                │
  │  node03 (worker)                │
  └─────────────────────────────────┘
        │
        │  routing
        ▼
    Traefik (Ingress)
        │
        ├── Longhorn (persistent storage)
        └── Prometheus + Grafana (monitoring)
```

## Repository Structure

```
homelab/
├── apps/                   # Application deployments
│   └── nginx/              # Example nginx deployment
├── infrastructure/         # Cluster infrastructure
│   ├── argocd/             # ArgoCD ingress configuration
│   └── monitoring/         # Grafana ingress configuration
└── README.md
```

## GitOps Workflow

All changes to the cluster are made through Git. Pushing to the `main` branch automatically triggers ArgoCD to sync the cluster state.

```
git push → ArgoCD detects change → deploys to cluster
```

## Applications

| App | Description |
|-----|-------------|
| nginx | Test deployment |
| Grafana | Monitoring dashboards |
| ArgoCD | GitOps UI |

## Monitoring

The cluster runs the full kube-prometheus-stack, which includes:
- **Prometheus** for scraping and storing metrics from all nodes and pods
- **Grafana** with pre-built Kubernetes dashboards for cluster, node, and pod visibility
- **Alertmanager** for alerting
- **Node Exporter** for hardware-level metrics from each node

## Remote Access

Remote administration is handled via [Tailscale](https://tailscale.com), allowing secure access to the cluster API and web interfaces from anywhere without exposing ports to the internet.

## Roadmap

- [ ] cert-manager + Let's Encrypt (automatic HTTPS)
- [ ] Cloudflare Tunnel / public app hosting
- [ ] CI/CD pipeline for custom applications
- [ ] Hugo blog deployment
- [ ] Homepage dashboard
- [ ] Gateway API (replacing Ingress)
