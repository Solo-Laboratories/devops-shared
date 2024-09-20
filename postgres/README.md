# Coder v2
Coder provides workspaces, using terraform templates, for isolated development in containers from a web interface.

## Items
* [values.yaml](db-values.yaml) file used for installing the database with Helm w/ default entries.
* [application.yaml](db-application.yaml) file used with ArgoCD to automate management of the service's database.
* [certificate.yaml](certificate.yaml) file used to issue the TLS certificate using [Cert Manager](../cert-manager/README.md)

## Prerequisites
* [Helm v3](https://helm.sh/docs/intro/install/)
* [Kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl)
* [ArgoCD](../argocd/README.md)
* Expose the postgres port for [traefik](../traefik/README.md) and redeploy Traefik.
* IFF cert manager is installed, get the certificate for the postgres database. `kubectl apply -f certificate.yaml` after making modifications to the certificate file.

## Installation
### ArgoCD
1. Make modifications to [application.yaml](application.yaml) to fit your environment.
2. Install the application and wait for the database to appear.
```bash
kubectl apply -f application.yaml
```

### Helm
1. Modify the [values.yaml](values.yaml) file to suit your environment. You can store the values file locally or remotely in a private repository.
2. Add database chart to Helm.
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```
3. Install the database using Helm. Either leave out the `--version` parameter OR change the value to match the **CHART** version you require.
```bash
helm upgrade --install postgres-db bitnami/postgresql -n postgres --create-namespace -f values.yaml --version 15.5.24 --atomic
```

## Updating
### ArgoCD
Update the application file for the service/database to use the version you want OR modify the Application in the WebUI by editing the details of the Application and change the chart version to the version you want to upgrade to.
### Helm
Replay the installation steps above.

## Uninstalling
### ArgoCD
Uninstalling the application files directly does not function currently. You have to navigate to the Applications in the ArgoCD and delete the applications from there. Select 'Foreground'(preferred) or 'Background' to remove the resources OR 'Non-cascading' if you wish to uninstall the application but leave the services there. Start with the Coder Application and then move to the Database application.

### Helm
1. Uninstall the chart using Helm.
```bash
helm uninstall postgres-db -n postgres
```