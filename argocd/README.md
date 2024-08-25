# WIP: UNTESTED INSTALLATION
# ArgoCD
ArgoCD is automates the management of Helm charts; installing, deleting, updating, modifying, etc. Pair with Git to get the most out of it.

## Items
* [values.yaml](values.yaml) file used for installing with Helm w/ default entries.

## Prerequisites
* [Helm v3](https://helm.sh/docs/intro/install/)
* [Kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl)

## Installation
1. Modify the [values.yaml](values.yaml) file to suit your environment. You can store this values file locally or remotely in a private repository.
2. Add service chart to Helm.
```bash
helm repo add argo https://argoproj.github.io/argo-helm
```
3. Install the service using Helm. Either leave out the `--version` parameter OR change the value to match the **CHART** version you require.
```bash
helm upgrade --install argo-cd argo/argo-cd -n argo-cd --create-namespace -f values.yaml --version=7.4.5 --atomic
```

## Uninstalling
### Helm
1. Uninstall the chart using Helm.
```bash
helm uninstall argo-cd -n argo-cd
```