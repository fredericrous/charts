---
name: Release Checklist
about: Checklist for releasing a new version of the Helm chart
title: 'Release v0.0.0'
labels: release
assignees: ''
---

## Release Checklist for v0.0.0

### Pre-release

- [ ] All tests passing on main branch
- [ ] CHANGELOG.md updated with all changes
- [ ] Documentation updated if needed
- [ ] Example configurations tested

### Chart Updates

- [ ] Update `version` in `Chart.yaml` to match release version (without 'v' prefix)
- [ ] Update `appVersion` in `Chart.yaml` if operator version changed
- [ ] Verify chart templates successfully with new version
- [ ] Run `helm lint ./charts/vault-transit-unseal-operator`

### Testing

- [ ] Test chart installation from local source
- [ ] Test chart upgrade from previous version
- [ ] Test with different values combinations:
  - [ ] Default values
  - [ ] High availability (replicaCount > 1)
  - [ ] Monitoring enabled
  - [ ] Network policies enabled
  - [ ] Custom resource limits

### Release

- [ ] Create git tag: `git tag v0.0.0`
- [ ] Push tag: `git push origin v0.0.0`
- [ ] Verify GitHub Actions workflow starts
- [ ] Monitor workflow for any failures

### Post-release

- [ ] Verify chart appears in charts repository index
- [ ] Test installation from charts repository:
  ```bash
  helm repo add vault-transit-unseal https://fredericrous.github.io/charts
  helm repo update
  helm search repo vault-transit-unseal/vault-transit-unseal-operator
  ```
- [ ] Verify GitHub Release created with chart package
- [ ] Update any dependent repositories or documentation
- [ ] Announce release (if applicable)

### Rollback Plan

If issues are discovered:
- [ ] Document the issue
- [ ] Delete the git tag: `git tag -d v0.0.0 && git push origin :refs/tags/v0.0.0`
- [ ] Fix the issue
- [ ] Re-run the release process

---

/cc @fredericrous