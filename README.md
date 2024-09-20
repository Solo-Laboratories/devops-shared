# DevOps Shared
Shared DevOps resources targeting Kubernetes, Docker, ArgoCD, and more.

## Migrating Helm to ArgoCD
Sometimes we want to migrate a helm chart to ArgoCD application to manage. Otherwise Helm and ArgoCD will fight. Just simply get all the secrets with helm in the name in the chart's namespace and delete them. Afterwards, apply the applcation.yaml file for ArgoCD to start taking over the automated updating. Super easy!

For example, assume I am wanting to migrate traefik chart which I installed in the traefik namespace.
```bash
kubectl get secret traefik -n traefik | grep helm
```
Then just `kubectl delete secret -n traefik <insert-secret-name>.v<some-version-number>`. Super easy!

## How to Install?
Follow the read me's in the subdirectories on how to install. Each service will be different.
* [ArgoCD](argocd/README.md)
* [Coder V2](coder/README.md)
* [Democratic CSI](democratic-csi/README.md)
* [Traefik](traefik/README.md)
* [NextCloud](nextcloud/README.md)
* [Oliven Tin](olivetin/README.md)
* [Home Assistant](home-assistant/README.md)
* [Cronjob Collections](cronjobs/README.md)
* [External Redirects](redirects/README.md)
* [Apache Guacamole](guacamole/README.md)
* [IngressRoute Collections](ingress-routes/README.md)
* [Gitea](gitea/README.md)
* [Excalidraw](excalidraw/README.md)
* [Cert Manager](cert-manager/README.md)
* [Postgres](postgres/README.md)