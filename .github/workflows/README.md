# GitHub Actions Workflows for Helm Charts Repository

This directory contains GitHub Actions workflows that automate the testing, packaging, and publishing of Helm charts in this multi-chart repository.

## Repository Structure

This is a multi-chart repository that can host multiple Helm charts. The structure is:

```
.
├── .github/
│   ├── workflows/         # CI/CD workflows
│   ├── yamllint.yaml      # YAML linting configuration
│   └── ct.yaml           # Chart testing configuration
├── README.md
├── index.yaml            # Helm repository index (auto-generated)
├── charts/               # Chart sources
│   ├── vault-transit-unseal-operator/
│   │   ├── Chart.yaml
│   │   ├── values.yaml
│   │   ├── templates/
│   │   └── ...
│   └── another-chart/    # Future charts
└── packages/             # Packaged charts (auto-generated)
    └── *.tgz
```

## Workflows

### 1. Auto Tag on Merge (`auto-tag-on-merge.yaml`)

**Trigger**: When a PR with `chart-update` label is merged

**Purpose**: Automatically creates release tags for chart updates

**Features**:
- Detects chart name and version from PR branch name (format: `update-<chart>-<version>`)
- Creates appropriate git tag (format: `<chart>-v<version>`)
- Posts confirmation comment on the merged PR
- Triggers the helm-release workflow automatically

**Note**: This workflow ensures the charts repository is fully automated - no manual tagging required!

### 2. Release Helm Chart (`helm-release.yaml`)

**Trigger**: 
- Git tags matching `chartname-v*.*.*` pattern (e.g., `vault-transit-unseal-operator-v0.1.0`)
- Manual workflow dispatch with chart selection

**Purpose**: Packages and publishes Helm charts

**Features**:
- Multi-chart support with automatic chart detection from tags
- Validates chart version matches git tag
- Comprehensive chart validation and testing
- Tests installation in Kind cluster
- Packages chart and updates repository index
- Creates GitHub release with the packaged chart
- Publishes to GitHub Pages

**Tag Format**:
- `<chart-name>-v<version>` (e.g., `vault-transit-unseal-operator-v0.1.0`)
- Chart name must match the directory name
- Version must match the version in Chart.yaml

### 2. Test Helm Charts (`helm-test.yaml`)

**Trigger**: 
- Pull requests modifying chart files
- Pushes to main branch modifying chart files

**Purpose**: Validates and tests all changed charts

**Features**:
- Automatically detects changed charts
- Parallel testing of multiple charts
- Chart linting with ct (chart-testing)
- Documentation validation
- Security scanning with Kubesec
- Template validation with multiple scenarios
- Installation testing on Kubernetes 1.28, 1.29, and 1.30
- Upgrade and uninstall testing

### 3. Update Helm Dependencies (`helm-deps-update.yaml`)

**Trigger**: 
- Weekly schedule (Mondays at 3 AM UTC)
- Manual workflow dispatch

**Purpose**: Automatically updates chart dependencies

**Features**:
- Scans all charts for outdated dependencies
- Updates dependency versions
- Increments chart patch version
- Creates PR with changes for review

## Setting Up the Repository

### 1. Repository Secrets

**Required Secrets:**

#### CHARTS_REPO_TOKEN (Required for releases)
This token is needed if you're publishing to a separate charts repository. Skip this if you're publishing to the same repository.

1. Create a Personal Access Token:
   - Go to GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)
   - Click "Generate new token"
   - Give it a descriptive name: `Charts Repository Write Access`
   - Select scopes:
     - `repo` (full control of private repositories)
   - Generate and copy the token

2. Add the secret to your repository:
   - Go to your repository Settings → Secrets and variables → Actions
   - Click "New repository secret"
   - Name: `CHARTS_REPO_TOKEN`
   - Value: Paste your personal access token
   - Click "Add secret"

**Optional Secrets:**

- `COSIGN_PRIVATE_KEY`: Private key for signing charts with Cosign
- `COSIGN_PASSWORD`: Password for the Cosign private key

### 2. GitHub Pages Configuration

1. Go to Settings → Pages
2. Source: Deploy from a branch
3. Branch: main
4. Folder: / (root)

### 3. Repository Settings

Enable the following:
- Actions: Read and write permissions
- Allow GitHub Actions to create pull requests

## Adding a New Chart

1. Create a new directory under `charts/` with your chart name:
   ```bash
   cd charts/
   mkdir my-new-chart
   cd my-new-chart
   helm create . --starter <optional-starter>
   ```

2. Ensure your chart has:
   - Proper `Chart.yaml` with version and description
   - Comprehensive `README.md`
   - Well-documented `values.yaml`

3. Test locally:
   ```bash
   helm lint charts/my-new-chart
   helm template test charts/my-new-chart
   ```

## Releasing a Chart

### Fully Automated Release (via External PR)

When a PR is created by external automation (e.g., from operator releases) with the `chart-update` label:

1. The PR is automatically tested via `helm-test.yaml`
2. Once merged, `auto-tag-on-merge.yaml` automatically:
   - Creates the appropriate release tag
   - Triggers `helm-release.yaml` to publish the chart

No manual intervention required!

### Manual Release

1. Update the chart version in `Chart.yaml`:
   ```yaml
   version: 0.2.0
   ```

2. Commit and push changes:
   ```bash
   git add charts/my-chart/Chart.yaml
   git commit -m "chore(my-chart): bump version to 0.2.0"
   git push
   ```

3. Create and push a tag:
   ```bash
   git tag my-chart-v0.2.0
   git push origin my-chart-v0.2.0
   ```

4. The workflow will automatically:
   - Validate the chart
   - Test installation
   - Package the chart
   - Update the repository index
   - Create a GitHub release

### Manual Release via Workflow Dispatch

Use the workflow dispatch feature:

1. Go to Actions → Release Helm Chart
2. Click "Run workflow"
3. Select:
   - Chart name
   - Version (e.g., v0.2.0)

## Best Practices

### 1. Version Management

- Use semantic versioning (MAJOR.MINOR.PATCH)
- Tag format: `<chart-name>-v<version>`
- Keep Chart.yaml version in sync with git tags

### 2. Testing

- Always run `helm lint` before committing
- Test installation locally with `helm install --dry-run`
- Add chart-specific test scenarios in values

### 3. Documentation

Each chart must have:
- `README.md` with installation and configuration instructions
- Comments in `values.yaml` explaining each option
- `CHANGELOG.md` tracking version changes

### 4. Security

- Run security scans locally before pushing
- Review Kubesec scan results in PR checks
- Keep dependencies up to date

## Troubleshooting

### Release workflow fails at version validation

Ensure your tag matches the Chart.yaml version:
```bash
# Check tag
git describe --tags

# Check Chart.yaml
grep version: charts/my-chart/Chart.yaml

# They should match (tag has 'v' prefix)
```

### Chart not appearing in repository

1. Check GitHub Pages is enabled
2. Verify workflow completed successfully
3. Wait 5-10 minutes for GitHub Pages to update
4. Check index.yaml in the repository

### Installation tests failing

1. Review the Kind cluster logs in the workflow
2. Check if your chart needs additional prerequisites
3. Ensure CRDs are in the `crds/` directory
4. Verify resource names don't conflict

## Local Development

### Testing workflows locally with act

```bash
# Install act
brew install act

# Test PR workflow
act pull_request -W .github/workflows/helm-test.yaml

# Test release workflow
act push -W .github/workflows/helm-release.yaml --secret-file .secrets
```

### Pre-commit hooks

Create `.pre-commit-config.yaml`:
```yaml
repos:
  - repo: https://github.com/norwoodj/helm-docs
    rev: v1.11.0
    hooks:
      - id: helm-docs
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.5.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
        args: [--allow-multiple-documents]
```