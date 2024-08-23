# Democratic CSI Values File
## Installation
Note that the IP Addresses below are localhost. Please change those - as well as the password for root - to what your TrueNAS is.
1. [Follow this guide](https://jonathangazeley.com/2021/01/05/using-truenas-to-provide-persistent-storage-for-kubernetes/) for NFS or iSCSI to setup your TrueNAS in preparation for Democratic CSI installation. Note that the values file is targetting ONLY NFS and thus should be configured differently using the values file or injection based on the guide linked above.
2. Install using the values file.
```bash
helm repo add democratic-csi https://democratic-csi.github.io/charts/ && \
helm repo update && \
helm upgrade --install democratic-csi democratic-csi/democratic-csi \
    --namespace csi \
    --create-namespace \
    --set driver.config.httpConnection.host=127.0.0.1 \
    --set driver.config.httpConnection.password="my-root-password" \
    --set driver.config.sshConnection.host=127.0.0.1 \
    --set driver.config.sshConnection.password="my-root-password" \
    --set driver.config.nfs.shareHost=127.0.0.1 \
    --version 0.14.6 \
    --atomic
```