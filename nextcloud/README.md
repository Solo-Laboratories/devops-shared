# Nextcloud Chart
Houses the umbrella chart that includes an IngressRoute for clusters using Traefik. 

## Installation

1. Install postgres bitnami chart first.
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami && \
helm repo update && \
helm upgrade --install nextcloud-db bitnami/postgresql \
    --namespace postgres \
    --create-namespace \
    --set auth.username=nextcloud \
    --set auth.password=nextcloud \
    --set auth.database=nextcloud \
    --set persistence.size=100Gi \
    --version v15.5.17 \
    --atomic
```

2. Install Coder Umbrella Chart. Take note of the postgres information you changed above and apply them below either during install (similar to below) or by modiying [the values file](coder/values.yaml).
```bash
helm dependency build && \
helm upgrade --install coder . \
    --namespace coder \
    --create-namespace \
    --set ingressRoute.url=coder.example.com \
    --set database.url=coder-db-postgresql.postgres.svc.cluster.local:5432 \
    --atomic
```

## Troubleshooting
* There might be an issue with installing and the default livenessprobe and readinessprobe. Disable them by `--set nextcloud.livenessProbe.enabled=false --set nextcloud.readinessProbe.enabled=false` on the first install with persistent data. This gives it more than enough time to install and setup.