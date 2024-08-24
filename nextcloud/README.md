# Nextcloud Chart
Houses the umbrella chart that includes an IngressRoute for clusters using Traefik. 

## Installation
1. Install Umbrella Chart. Take note of the postgres information you changed above and apply them to [the values file](coder/values.yaml).
```bash
helm repo add solo-laboratories https://solo-laboratories.github.io/helm-charts && \
helm upgrade --install nextcloud solo-laboratories/nextcloud-with-traefik \
    --namespace nextcloud \
    --create-namespace \
    --version v2.0.0 \
    --atomic
```

## Note
This chart does not use an external postgres in the sense that the postgres is located outside of the nextcloud namespace. It is, however, configured to stand up an external postgres database inside the nextcloud namespace.

## Troubleshooting
* There might be an issue with installing and the default livenessprobe and readinessprobe. Disable them by `--set nextcloud.livenessProbe.enabled=false --set nextcloud.readinessProbe.enabled=false` on the first install with persistent data. This gives it more than enough time to install and setup.