# Cert Manager
Cert Manager... well manages your certificates! All sorts of certificates! But mainly used to store and manage the certificates for your TLS endpoint.

## Notes:
* [Cert Manager docs](https://cert-manager.io/docs)
* [TechnoTim's Guide](https://technotim.live/posts/kube-traefik-cert-manager-le/)
* **!!WARNING!! USE STAGING BEFORE PRODUCTION FOR CERTS OR YOU RISK BEING BLOCKED FOR 7 days!**

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
helm repo add jetstack https://charts.jetstack.io
```
3. Install the service using Helm. Either leave out the `--version` parameter OR change the value to match the **CHART** version you require.
```bash
helm upgrade --install cert-manager jetstack/cert-manager -n cert-manager --create-namespace -f values.yaml --version v1.15.3 --atomic
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
helm uninstall cert-manager -n cert-manager
```

## Usage
### Getting a certificate
Getting a certificate is easy. 

1. You want to [get your cloudflar API token](https://cert-manager.io/docs/configuration/acme/dns01/cloudflare/#api-tokens) and replace it in the [token-secret.yaml](token-secret.yaml) file. Then apply the file.
```bash
kubectl apply -f token-secret.yaml
``` 
2. Fill out the email and domain in [staging-issuer.yaml](staging-issuer.yaml) file. Then apply the file.
```bash
kubectl apply -f staging-issuer.yaml
```
3. Change what you need in the [example staing cert](example-com-staging-cert.yaml) file. Then apply the file... and wait!
```bash
kubectl apply -f example-com-staging-cert.yaml && watch -d kubectl get challenges -A
```
4. Once the challenge passes, you will have your certificate. Assuming you didn't change much from the files above the staging secret would have been `domain-name-staging-tls` and you want to place it at `spec.tls.secretName` in your ingress route. However, if placed in traefik namespace, it seems Traefik automatically picks up the secret and applies the TLS certificate information thus not needing you to add the above to your ingress route manifest.
5. Finally, go to your endpoint. It will show that it is not protected but staging will do that. Just inspect the certificate in your browser and if you see that it has LetsEncrypt Staging in the details, you know it works. IFF you see staging in the details, then do the same process from step 2 but with production. You want delete the staging certificate you if you like. `kubectl delete certificate -n traefik example-com-staging`.