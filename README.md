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
| [vault-transit-unseal-operator](charts/vault-transit-unseal-operator/) | 2.0.2 | 2.0.2 | A Kubernetes operator that automatically manages HashiCorp Vault initialization and unsealing using transit unseal |

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
