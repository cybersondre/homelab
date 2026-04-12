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
| [Tailscale](https://tailscale.com) | Remote access VPN |

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
```

## Repository Structure

```
homelab/
├── apps/                   # Application deployments
│   └── nginx/              # Example nginx deployment
├── infrastructure/         # Cluster infrastructure
│   └── argocd/             # ArgoCD ingress configuration
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

## Roadmap

- [ ] Monitoring stack (Grafana + Prometheus)
- [ ] Persistent storage (Longhorn)
- [ ] CI/CD pipeline for custom applications
- [ ] Cloudflare Tunnel / Tailscale remote access
- [ ] Homepage dashboard
