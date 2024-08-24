# IngressRoutes Chart
Creates IngressRoute objects used with Traefik to route to services.

## Installation
Using Helm install the chart using the below example. Make to add what endpoints you need in the [values.yaml file](values.yaml)
```bash
helm upgrade --install ingress-routes . \
    --namespace routes \
    --create-namespace \
    --atomic
```
