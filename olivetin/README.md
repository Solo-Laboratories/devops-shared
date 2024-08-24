# Olivetin Chart
House the copied files from [Olivetin's helm chart repository](https://github.com/OliveTin/OliveTin-HelmChart/tree/main) to make modification changes to the helm chart.

## Installation
1. Install Umbrella Chart. Take note of the postgres information you changed above and apply them to [the values file](coder/values.yaml).
```bash
helm repo add solo-laboratories https://solo-laboratories.github.io/helm-charts && \
helm upgrade --install olivetin solo-laboratories/olivetin-with-traefik \
    --namespace olivetin \
    --create-namespace \
    --version v2.0.0 \
    --atomic
```