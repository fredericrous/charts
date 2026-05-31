# Helm Charts Repository

This repository contains Helm charts for various Kubernetes applications.

## Usage

```bash
helm repo add fredericrous https://fredericrous.github.io/charts
helm repo update
```

## Available Charts

| Chart | Version | App Version | Description |
|-------|---------|-------------|-------------|
| [builder-api](charts/builder-api/) | 0.0.8 | 0.0.8 | website-builder backend API (createSite, listMySites, getSite, publishCanned) |
| [duro-app](charts/duro-app/) | 1.43.0 | 1.43.0 | A household management dashboard with invite system |
| [workerd-runtime](charts/workerd-runtime/) | 0.0.8 | 0.0.8 | serves materialised HTML from garage, slug→site_id from Postgres |
| [builder-webapp](charts/builder-webapp/) | 0.0.34 | 0.0.34 | website-builder SPA + auth backend (openauth client + session cookie) |
| [social-planner](charts/social-planner/) | 0.1.8 | 0.1.8 | Self-hosted social content planner (recommendation + week planner) |
| [vault-transit-unseal-operator](charts/vault-transit-unseal-operator/) | 2.3.0 | 2.3.0 | A Kubernetes operator that automatically manages HashiCorp Vault initialization and unsealing using transit unseal |
| [kb-vision](charts/kb-vision/) | 0.11.0 | 0.11.0 | Knowledge Base for Homelab |
| [sync-bridge](charts/sync-bridge/) | 0.0.6 | 0.0.6 | Node service that bridges browser WebSocket clients to Hyperswarm for Yjs document sync in the website-builder. Single-pod for v1 (per PLAN.md "v1 scaling posture"). The Hyperswarm DHT layer requires UDP egress for hole-punching, so production deployments use `hostNetwork: true` or a NodePort with UDP forwarding — see values.yaml. |
| [openauth](charts/openauth/) | 0.0.4 | 0.0.4 | OIDC issuer for the website-builder (wraps @openauthjs/openauth) |
| [ticket-vision](charts/ticket-vision/) | 0.7.2 | 0.7.2 | Fully automated ITSM for homelab |
| [cluster-vision](charts/cluster-vision/) | 0.20.1 | 0.20.1 | Auto-generated infrastructure diagrams from live Kubernetes state |
| [authelia-oidc-operator](charts/authelia-oidc-operator/) | 0.1.27 | 0.1.27 | A Kubernetes operator that manages OIDC client configurations for Authelia |
| [duro-operator](charts/duro-operator/) | 0.1.5 | 0.1.5 | A Kubernetes operator that manages DashboardApp resources for the Duro dashboard |
| [ddns-updater-operator](charts/ddns-updater-operator/) | 0.4.1 | 0.4.1 | A Kubernetes operator that manages DDNS records for ddns-updater |
| [homelab-preview-operator](charts/homelab-preview-operator/) | 0.5.15 | 0.5.15 | A Kubernetes operator that automatically configures preview environments for applications |

## Installing a Chart

To install a chart from this repository:

```bash
# Add the repository
helm repo add fredericrous https://fredericrous.github.io/charts
helm repo update

# Install a chart
helm install my-release fredericrous/<chart-name>

# Install with custom values
helm install my-release fredericrous/<chart-name> -f my-values.yaml
```

## Development

### Repository Structure

```
.
├── .github/           # GitHub Actions workflows
├── README.md          # This file
├── index.yaml         # Helm repository index (auto-generated)
├── charts/            # Chart sources
│   ├── authelia-oidc-operator/
│   └── vault-transit-unseal-operator/
└── packages/          # Packaged charts (*.tgz)
```

### Adding a New Chart

1. Create a new directory under `charts/` for your chart
2. Follow the standard Helm chart structure
3. Include comprehensive documentation
4. Test the chart thoroughly
5. Submit a pull request

### Release Process

Charts are released automatically when a tag is pushed:

```bash
git tag <chart-name>-v<version>
git push origin <chart-name>-v<version>
```

For example:
```bash
git tag vault-transit-unseal-operator-v0.1.0
git push origin vault-transit-unseal-operator-v0.1.0
```

### Testing

All charts are automatically tested on:
- Kubernetes 1.28
- Kubernetes 1.29  
- Kubernetes 1.30

Tests include:
- Helm linting
- Template validation
- Installation testing
- Security scanning

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

### Guidelines

1. Follow Helm best practices
2. Include comprehensive README.md
3. Document all values in values.yaml
4. Add appropriate labels and annotations
5. Ensure charts are production-ready

## License

See individual chart directories for license information.
