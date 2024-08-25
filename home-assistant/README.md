# Home Assistant
Home Assistant is an automation service for automating home related gudgets such as lights, doors, cameras, etc and more!

## Items
* [values.yaml](values.yaml) file used for installing with Helm w/ default entries.
* [application.yaml](application.yaml) file used with ArgoCD to automate management of the service.

## Prerequisites
* [Helm v3](https://helm.sh/docs/intro/install/)
* [Kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl)
* [ArgoCD](../argocd/README.md)

## Installation
### ArgoCD
1. Make modifications to [application.yaml](application.yaml) file so it points to where you are storing your values.file.
2. Install the application and wait for the service to appear.
```bash
kubectl apply -f application.yaml
```

### Helm
1. Modify the [values.yaml](values.yaml) file to suit your environment. You can store this values file locally or remotely in a private repository.
2. Add service chart to Helm.
```bash
helm repo add home-hass http://pajikos.github.io/home-assistant-helm-chart
```
3. Install the service using Helm. Either leave out the `--version` parameter OR change the value to match the **CHART** version you require.
```bash
helm upgrade --install home-assistant home-hass/home-assistant -n home-assistant --create-namespace -f values.yaml --version 0.2.74 --atomic
```

## Updating
### ArgoCD
Update the application file for the service to use the version you want OR modify the Application in the WebUI by editing the details of the Application and change the chart version to the version you want to upgrade to.
### Helm
Replay the installation steps above.

## Uninstalling
### ArgoCD
Uninstalling the application file directly does not function currently. You have to navigate to the Application in the ArgoCD and delete the application from there. Select 'Foreground'(preferred) or 'Background' to remove the resources OR 'Non-cascading' if you wish to uninstall the application but leave the services there.

### Helm
1. Uninstall the chart using Helm.
```bash
helm uninstall home-assistant -n home-assistant
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