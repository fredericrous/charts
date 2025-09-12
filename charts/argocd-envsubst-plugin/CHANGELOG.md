# Changelog

## [2.1.0] - 2024-01-09

### Added
- Support for .env files in repository
- Environment-specific configuration loading
- Custom .env file paths via ENV_FILE

### Changed
- Updated plugin to v2.1.0
- Improved documentation

## [2.0.1] - 2024-01-09

### Fixed
- Docker build issues with start.sh
- Helm chart linting errors
- Container permission issues

### Changed
- Updated plugin to v2.0.1

## [2.0.0] - 2024-01-09

### BREAKING CHANGES
- Removed auto-discovery feature - plugin must now be explicitly specified in Application manifests

### Changed
- Plugin now follows ArgoCD best practices
- Updated to Config Management Plugin v2 specification

## [1.0.0] - 2024-01-08

### Added
- Initial release
- Environment variable substitution for Kustomize manifests
- Support for ${VAR} and ${VAR:-default} syntax
- Metrics endpoint
- Caching support