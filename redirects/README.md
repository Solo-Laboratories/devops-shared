# External Redirects Chart
Allows for redirects to external services using Traefik

## Installation
Using Helm install the chart using the below example. Make to add what external endpoints you need in the [values.yaml file](values.yaml)
```bash
helm upgrade --install external-redirects . \
    --namespace redirects \
    --create-namespace \
    --atomic
```
