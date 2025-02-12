# kube-check-action

GitHub composite action to run kustomize, kubeconform, kube-linter and pluto

## Inputs

See `action.yml` for up to date documentation.

```yaml
inputs:
  kustomize_version:
    description: kustomize version
    required: false
    default: "5.6.0"
  kubeconform_version:
    description: kubeconform version
    required: false
    default: "0.6.7"
  kube-linter_version:
    description: kube-linter version
    required: false
    default: "0.7.1"
  pluto_version:
    description: pluto version
    required: false
    default: "5.21.1"
  kustomize_build_input:
    description: 'Input parameter for kustomize build'
    required: true
  kubeconform_flags:
    description: flags to supply to kubeconform
    required: false
  kube-linter_flags:
    description: flags to supply to kube-linter lint
    required: false
```

## Example Usage

```yaml
name: Check kubernetes manifests

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  check:
    name: Lint kubernetes manifests
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Build and validate kustomizations
        uses: ubergesundheit/kube-check-action@main
        with:
          kustomize_build_input: deployment/production
          kubeconform_flags: "-strict -kubernetes-version 1.22.7"
```

## Validating sops encrypted Secrets

Add `-schema-location 'https://raw.githubusercontent.com/ubergesundheit/kube-check-action/main/kubeconform-schemas/{{.ResourceKind}}.json' -schema-location default` to your `kubeconform_flags`
