---
canonical: fredericrous/adr-lint docs/plans/2026-11-08-ci-fetches-release-assets-api.md
phases: [2]
status: done
branch: fix/gh-api-downloads
repos: [charts]
---

# CI fetches GitHub release assets through the API, fleet-wide (pointer)

This repository's slice of the fleet plan: Phase 2. The plan, its review
and its recipe are canonical in adr-lint (pilot: fredericrous/adr-lint#53).

## Code review

- charts: backend, platform

## Outcome

- input: the lookup with a real token, `CURRENT_VERSION=v0.18.0` → expected: the latest tag read, bump decided → actual: "kube-rbac-proxy: current v0.18.0, latest v0.23.1", BUMP.
- input: the same with `GH_TOKEN=bogus` → expected: a warning and no bump → actual: `::warning::kube-rbac-proxy latest-release lookup failed; bump skipped …`, "latest unknown", SKIP.
- input: repository-wide grep → expected: only non-goals → actual: icon URLs in `index.yaml` and `charts/vault-transit-unseal-operator/Chart.yaml:19` (chart metadata, nothing downloads them in CI).
- Not dispatched before the merge: a dispatch opens a real bump PR. The first scheduled run is the CI observation; check that a bumped tag exists at the chart's registry (gcr.io vs quay.io, raised by backend and platform) before merging such a PR.

## Implementation review

- charts implementation-review: approve-with-changes (low: warn on a failed lookup, fixed; low: outcome, done here), then approve on the Delta; 38k+23k, 31+9 s (opus).
- charts backend: approve-with-changes (medium: silent failure, fixed; medium: no pre-merge run, deliberate because a dispatch opens a real PR; low: tag shape, fixed), then approve; 29k+23k, 29+21 s (opus).
- charts platform: approve (low: the same silent failure, fixed in the next commit; its pass carries by ancestry); 32k, 29 s (opus).
