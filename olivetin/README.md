# Olivetin Chart
House the copied files from [Olivetin's helm chart repository](https://github.com/OliveTin/OliveTin-HelmChart/tree/main) to make modification changes to the helm chart.

## Installation
1. Install Olivetin. Folow the instructions below (assuming the command is ran at {git_repo_root}/olivetin).
```bash
helm upgrade --install olivetin . \
    --namespace olivetin-ns \
    --create-namespace \
    --set ingressRoute.url=olivetin.example.com \
    --atomic
```