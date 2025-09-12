# ArgoCD Envsubst Plugin Helm Chart

This chart installs the ArgoCD envsubst plugin as a sidecar container in ArgoCD.

## TL;DR

```bash
helm repo add fredericrous https://fredericrous.github.io/charts
helm install argocd-envsubst-plugin fredericrous/argocd-envsubst-plugin \
  --namespace argocd
```

## Introduction

This chart bootstraps the [ArgoCD envsubst plugin](https://github.com/fredericrous/argocd-plugin-envsubst) deployment on a Kubernetes cluster using the Helm package manager.

## Prerequisites

- Kubernetes 1.19+
- Helm 3.2.0+
- ArgoCD 2.5.0+ installed

## Installing the Chart

To install the chart with the release name `argocd-envsubst-plugin`:

```bash
helm install argocd-envsubst-plugin fredericrous/argocd-envsubst-plugin \
  --namespace argocd
```

## Uninstalling the Chart

To uninstall/delete the `argocd-envsubst-plugin` deployment:

```bash
helm uninstall argocd-envsubst-plugin -n argocd
```

## Configuration

The following table lists the configurable parameters of the ArgoCD envsubst plugin chart and their default values.

| Parameter | Description | Default |
|-----------|-------------|---------|
| `replicaCount` | Number of replicas | `1` |
| `testMode` | Enable test mode (skips deployment for chart testing) | `false` |
| `image.repository` | Image repository | `ghcr.io/fredericrous/argocd-plugin-envsubst` |
| `image.tag` | Image tag (defaults to chart appVersion) | `""` |
| `image.pullPolicy` | Image pull policy | `IfNotPresent` |
| `imagePullSecrets` | Image pull secrets | `[]` |
| `nameOverride` | String to partially override fullname | `""` |
| `fullnameOverride` | String to fully override fullname | `""` |
| `serviceAccount.create` | Create a service account | `false` |
| `serviceAccount.annotations` | Service account annotations | `{}` |
| `serviceAccount.name` | Service account name | `""` |
| `podAnnotations` | Pod annotations | `{}` |
| `podLabels` | Pod labels | `{}` |
| `podSecurityContext` | Pod security context | `{}` |
| `securityContext` | Container security context | See values.yaml |
| `resources` | CPU/Memory resource requests/limits | See values.yaml |
| `env` | Environment variables | `{}` |
| `envFrom` | Environment variables from secrets/configmaps | `[]` |
| `nodeSelector` | Node selector | `{}` |
| `tolerations` | Tolerations | `[]` |
| `affinity` | Affinity | `{}` |

### Using with ConfigMap

```yaml
# values.yaml
envFrom:
- configMapRef:
    name: argocd-env-vars
```

### Using with Secret

```yaml
# values.yaml
envFrom:
- secretRef:
    name: argocd-env-secret
```

## Usage

Once installed, you can use the plugin in your ArgoCD Application manifests:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
spec:
  source:
    plugin:
      name: envsubst
```

For more information on using the plugin, see the [plugin documentation](https://github.com/fredericrous/argocd-plugin-envsubst).