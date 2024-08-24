# Apache Guacamole Chart
Houses the umbrella chart that includes an IngressRoute for clusters using Traefik. 

## Installation
1. Install postgres bitnami chart first.
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami && \
helm repo update && \
helm upgrade --install guacamole-db bitnami/postgresql \
    --namespace postgres \
    --create-namespace \
    --set auth.username=guacamole \
    --set auth.password=guacamole \
    --set auth.database=guacamole \
    --set persistence.size=4Gi \
    --version v15.5.17 \
    --atomic
```
2. Install Coder Umbrella Chart. Take note of the postgres information you changed above and apply them to [the values file](coder/values.yaml).
```bash
helm repo add solo-laboratories https://solo-laboratories.github.io/helm-charts && \
helm upgrade --install guacamole solo-laboratories/guacamole-with-traefik \
    --namespace guacamole \
    --create-namespace \
    --version v2.0.0 \
    --atomic
```

*Note: The default user is 'guacadmin' and the password is 'guacadmin'*