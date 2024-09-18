# Traefik Chart
This chart is designed to function with K3s Traefik and works with HelmChartConfig resources.

## !!WARNING!!
This chart is for installing traefik align side k3s! If you want to install as stand alone, please apply the application.yaml file with the standalone_values.yaml file. Note, this does not include the traefik endpoint. You will need to add the ingress route by yourself. 

Additionally, the stand alone - as is - is configured to be used with [cert-manager](../cert-manager/README.md). It is HIGHLY suggested you follow the cert-manager guide for installing as well as applying the cert-manager application. 

## !!WARNING!!
This chart will not persist unless you follow [these steps](#persistance). This chart also assumes traefik is already installed AND you're using `HelmChartConfig` resources. If you've installed K3s, you should be all good to use this chart as soon as the cluster has been installed.

## Prep work
There are multiple things you must do before installing this chart.

* Have a registered domain you want to use.
* Choose your provider. This chart uses cloudflare. Follow [TechnoTims guide](https://technotim.live/posts/traefik-portainer-ssl/) for setting up your domain with cloudflare and get your API Token. Just follow the guide until Cloudflare is setup.
* Get your API Token from Cloudflare, storing for later use.
* Decide upon what your endpoint will be for Traefik dashboard IF you enable it.
* Have a CSI provisioner; I suggest Longhorn CSI if you're wanting to use distributed local storage (local storage the cluster is running on) or [Democratic CSI](../democratic-csi/README.md)

## Installation
Follow the steps below to install the charts. For now I assume local install though a remote chart is a feature that will be coming later on.

1. Modify any configuration changes in the [values.yaml file](values.yaml).
2. Install the chart.
3. (Optional BUT recommended!) Configure [Persistance](#persistance).

After modifying the [values.yaml](values.yaml) file with personalized changes, do the following command to install the traefik items.

(Step 2)
```bash
helm repo add solo-laboratories https://solo-laboratories.github.io/helm-charts && \
helm upgrade --install traefik-custom solo-laboratories/traefik-custom \
    --namespace kube-system \
    --version v1.0.0 \
    --atomic
```

## Persistance
The above chart will function but parts will not persist past a cluster restart. Follow these steps to make the items persist through cluster restarts.

1. Generate the `HelmChartConfig` based on your [values.yaml](values.yaml) file.
2. Place the new file on your control node.
3. Profit!

Those steps seem straight forward. Here those steps are in bash form!
Step 1:
```bash
helm template name -n kube-system -s templates/helm-config.yml > traefik-custom.yml
```

Step 2:
```bash
scp traefik-custom.yml <service-ip>: && \
ssh <service-ip> sudo mv traefik-custom.yml /var/rancher/k3s/server/manifests/
```
Step 3:
Persistance through cluster reboots!
