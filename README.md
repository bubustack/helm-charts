# BubuStack Helm Charts

[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/bubustack)](https://artifacthub.io/packages/search?repo=bubustack)

This repository contains the Helm charts published for the BubuStack projects:

- `bobrapet` – the workflow operator
- `bobravoz-grpc` – the transport hub
- `bubuilder` - BubuStack UI

Charts are stored under `charts/<name>` and should not be edited manually. They are generated
from the corresponding operator repositories through the release workflows and arrive here via
automated pull requests.

## Release pipeline

1. A new release is cut in `bobrapet`, `bobravoz-grpc`, or `bubuilder` via Release Please.
2. The `sync-helm-chart` job stamps the release version into `Chart.yaml` and `values.yaml`,
   runs `helmify`, and opens a pull request in this repository.
3. When the pull request is merged into `main`, the `Publish Helm Charts` workflow
   packages the charts, updates the GitHub Pages index via `helm/chart-releaser`,
   and pushes OCI artifacts to `ghcr.io/bubustack/charts`.

## Installing the charts

### From the Helm repository (GitHub Pages)

```bash
helm repo add bubustack https://bubustack.github.io/helm-charts
helm repo update

helm install bobrapet bubustack/bobrapet \
  --namespace bobrapet-system --create-namespace

helm install bobravoz-grpc bubustack/bobravoz-grpc \
  --namespace bobrapet-system

helm install bubuilder bubustack/bubuilder \
  --namespace bobrapet-system
```

### From the OCI registry (GHCR)

```bash
helm install bobrapet oci://ghcr.io/bubustack/charts/bobrapet \
  --version <chart-version> \
  --namespace bobrapet-system --create-namespace
```

### From Artifact Hub

Browse and install charts from [artifacthub.io/packages/search?repo=bubustack](https://artifacthub.io/packages/search?repo=bubustack).

## Local development

- Keep long-lived defaults (e.g., tuned `values.yaml`) inside the source repositories under `hack/charts/<name>`.
- Let automation regenerate templates — only review and merge the resulting PRs here.
- Chart versions and `appVersion` are stamped automatically from the operator release tag.
  No manual version bumps needed.

## Links

- [Documentation](https://bubustack.io/docs)
- [Operator Configuration](https://bubustack.io/docs/operator/configuration)
- [Getting Started](https://bubustack.io/docs/getting-started/quickstart)
- [GitHub](https://github.com/bubustack/bobrapet)
- [Discord](https://discord.gg/dysrB7D8H6)
