# External Redirects Chart
Allows for redirects to external services using Traefik

## Installation
1. Install Umbrella Chart. Take note of the postgres information you changed above and apply them to [the values file](coder/values.yaml).
```bash
helm repo add solo-laboratories https://solo-laboratories.github.io/helm-charts && \
helm upgrade --install redirects solo-laboratories/external-redirects \
    --namespace redirects \
    --create-namespace \
    --version v1.0.0 \
    --atomic
```
