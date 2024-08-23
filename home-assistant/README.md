# Home Assistant Chart with Traefik Ingress
Houses Home Assistant Helm umbrella chart that includes an ingress route for those using Traefik.

## Installation
1. Install Home Assistant. Folow the instructions below (assuming the command is ran at {git_repo_root}/home-assistant). Change any values you need in the values file based on [charts default values file](https://artifacthub.io/packages/helm/helm-hass/home-assistant) under `home-assistant`
```bash
helm dependency build && \
helm upgrade --install home-assistant . \
    --namespace home-assistant \
    --create-namespace \
    --set ingressRoute.url=olivetin.example.com \
    --atomic
```

## Known Issues
* https://github.com/pajikos/home-assistant-helm-chart/issues/30
Unfortunately, the helm chart used does not properly mount the trust_proxies (when using traefik) so home-assistant fails to work. Do the follow below to fix this issue.

```bash
kubectl exec -it -n home-assistant pod/home-assistant-0 -- bash -c "vi configuration.yaml"
```

Then modify the entry below the `http` to add the trusted proxy IP address. If you're using Traefik in k3s, this is what it should looke lik.

```yaml
# Loads default set of integrations. Do not remove.
default_config:

http:
    use_x_forwarded_for: true
    trusted_proxies:
    - 10.42.0.0/16
# Load frontend themes from the themes folder
frontend:
themes: !include_dir_merge_named themes

automation: !include automations.yaml
script: !include scripts.yaml
scene: !include scenes.yaml
```

Then just restart the deployment
```bash
kubectl rollout restart statefulset/home-assistant -n home-assistant
```


* After making a new user, the page fails to load on the auth callback. Just reload base url, relog in, and you have acces to your home assistant board