# Nextcloud
Nextcloud provides Web related Google Applications as FOSS such Drive, Photos, Storage, etc

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
helm repo add nextcloud https://nextcloud.github.io/helm/
```
3. Install the service using Helm. Either leave out the `--version` parameter OR change the value to match the **CHART** version you require.
```bash
helm upgrade --install nextcloud nextcloud/nextcloud -n nextcloud --create-namespace -f values.yaml --version 5.5.2 --atomic
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
helm uninstall nextcloud -n nextcloud
```

## Note
This chart does not use an external postgres in the sense that the postgres is located outside of the nextcloud namespace. It is, however, configured to stand up an external postgres database inside the nextcloud namespace.

## Troubleshooting
* There might be an issue with installing and the default livenessprobe and readinessprobe. Disable them by `--set nextcloud.livenessProbe.enabled=false --set nextcloud.readinessProbe.enabled=false` on the first install with persistent data. This gives it more than enough time to install and setup.