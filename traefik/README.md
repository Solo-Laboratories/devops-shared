# Traefik Chart
This chart is designed to function with K3s Traefik and works with HelmChartConfig resources.

## !!WARNING!!
This installation is configured to be used with [cert-manager](../cert-manager/README.md). It is HIGHLY suggested you follow the cert-manager guide for installing as well as applying the cert-manager application. 

## Items
* [values.yaml](values.yaml) file used for installing with Helm w/ default entries.
* [application.yaml](application.yaml) file used with ArgoCD to automate management of the service.

## Prerequisites
* [Helm v3](https://helm.sh/docs/intro/install/)
* [Kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl)
* [ArgoCD](../argocd/README.md)

## Installation
### ArgoCD
1. Make modifications to [application.yaml](application.yaml) file so it points to where you are storing your values.file.
2. Install the application and wait for the service to appear.
```bash
kubectl apply -f application.yaml
```

### Helm
1. Modify the [values.yaml](values.yaml) file to suit your environment. You can store this values file locally or remotely in a private repository.
2. Add service chart to Helm.
```bash
helm repo add traefik https://helm.traefik.io/traefik
```
3. Install the service using Helm. Either leave out the `--version` parameter OR change the value to match the **CHART** version you require.
```bash
helm upgrade --install traefik traefik/traefik -n traefik --create-namespace -f values.yaml --version 31.0.0 --atomic
```

## Updating
### ArgoCD
Update the application file for the service to use the version you want OR modify the Application in the WebUI by editing the details of the Application and change the chart version to the version you want to upgrade to.
### Helm
Replay the installation steps above.

## Uninstalling
### ArgoCD
Uninstalling the application file directly does not function currently. You have to navigate to the Application in the ArgoCD and delete the application from there. Select 'Foreground'(preferred) or 'Background' to remove the resources OR 'Non-cascading' if you wish to uninstall the application but leave the services there.

### Helm
1. Uninstall the chart using Helm.
```bash
helm uninstall traefik -n traefik
```