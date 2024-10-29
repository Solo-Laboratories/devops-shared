# Similar to Draw.io

## Installation
_Chart link:_ https://artifacthub.io/packages/helm/excalidraw/excalidraw

```bash
helm repo add excalidraw https://pmoscode-helm.github.io/excalidraw/ && \
helm repo update && \
helm upgrade --install excalidraw excalidraw/excalidraw -n excalidraw -f values.yaml --create-namespace --atomic
```

## Updating
Unfortunately, Excalidraw uses 'latest' for the docker container and doesn't use any versioning. This means, we have to uninstall and reinstall the chart to "update" to latest version. This won't be picked up by ArgoCD so there is no reason to make an application for it. Just uninstall and re-install at your own pleasure.

```bash
helm uninstall excalidraw -n excalidraw && \
helm upgrade --install excalidraw excalidraw/excalidraw -n excalidraw -f values.yaml --create-namespace --atomic
```