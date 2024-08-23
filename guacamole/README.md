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
2. Install Guacamole Umbrella Chart. Take note of the postgres information you changed above and apply them below either during install (similar to below) or by modiying [the values file](values.yaml).
```bash
helm dependency build && \
helm upgrade --install guacamole . \
    --namespace guacamole \
    --create-namespace \
    --set ingressRoute.url=guacamole.example.com \
    --atomic
```

_Note: The default user is 'guacadmin' and the password is 'guacadmin'