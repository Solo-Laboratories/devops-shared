# Apache Guacamole 
Guacamole provides Administration of resources (Hardware, VMs, etc.) from the Web.

## Items
* [values.yaml](values.yaml) file used for installing with Helm w/ default entries.
* [application.yaml](application.yaml) file used with ArgoCD to automate management of the service.
* [db-values.yaml](db-values.yaml) file used for installing the database with Helm w/ default entries.
* [db-application.yaml](db-application.yaml) file used with ArgoCD to automate management of the service's database.

## Prerequisites
* [Helm v3](https://helm.sh/docs/intro/install/)
* [Kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl)
* [ArgoCD](../argocd/README.md)

## Installation
### ArgoCD
1. Make modifications to [application.yaml](application.yaml) and [db-application.yaml](db-application.yaml) files to fit your environment.
2. Install the db-application and wait for the database to appear.
```bash
kubectl apply -f db-application.yaml
```
3. Install the db-application and wait for the service to appear.
```bash
kubectl apply -f application.yaml
```

### Helm
1. Modify the [db-values.yaml](db-values.yaml) file to suit your environment. You can store the values file locally or remotely in a private repository.
2. Add database chart to Helm.
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```
3. Install the database using Helm. Either leave out the `--version` parameter OR change the value to match the **CHART** version you require.
```bash
helm upgrade --install guacamole-db bitnami/postgresql -n postgres --create-namespace -f db-values.yaml --version 15.5.24 --atomic
```
4. Modify the [values.yaml](values.yaml) file to suit your environment. You can store the values file locally or remotely in a private repository. Note that if change the user, password, or database in [db-values.yaml](db-values.yaml), you will need to reflect those changes in this values file.
5. Add service chart to Helm.
```bash
helm repo add guacamole https://charts.beryju.org
```
6. Install the service using Helm. Either leave out the `--version` parameter OR change the value to match the **CHART** version you require.
```bash
helm upgrade --install guacamole guacamole/guacamole -n guacamole --create-namespace -f values.yaml --version 1.4.1 --atomic
```

## Updating
### ArgoCD
Update the application file for the service/database to use the version you want OR modify the Application in the WebUI by editing the details of the Application and change the chart version to the version you want to upgrade to.
### Helm
Replay the installation steps above.

## Uninstalling
### ArgoCD
Uninstalling the application files directly does not function currently. You have to navigate to the Applications in the ArgoCD and delete the applications from there. Select 'Foreground'(preferred) or 'Background' to remove the resources OR 'Non-cascading' if you wish to uninstall the application but leave the services there. Start with the Service Application and then move to the Database application.

### Helm
1. Uninstall the chart using Helm.
```bash
helm uninstall guacamole -n guacamole
```
2. Uninstall the database using Helm.
```bash
helm uninstall guacamole-db -n postgres
```