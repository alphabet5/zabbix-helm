# zabbix-helm
 Helm Chart for zabbix installation.

## Installation

This documentation has been tested with microk8s. MetalLB is used as the load balancer.

```bash
microk8s enable storage
microk8s enable metallb
microk8s enable dns
sudo systemctl enable iscsid
microk8s enable openebs
```


### Install the dependencies
Note: These are not installed automatically (yet.)

```bash
helm repo add sealed-secrets https://bitnami-labs.github.io/sealed-secrets
helm repo add stakater https://stakater.github.io/stakater-charts
helm repo add cert-manager https://charts.jetstack.io
```

```bash
helm install cert-manager jetstack/cert-manager --namespace cert-manager --create-namespace --set installCRDs=true --version v1.7.0
```

```bash
helm install sealed-secrets sealed-secrets/sealed-secrets --namespace sealed-secrets --set installCRDs=true --create-namespace --version 2.1.2
```

```bash
helm install reloader stakater/reloader --namespace reloader --create-namespace --set installCRDs=true
```

### Generate Encrypted Secrets

```bash
echo -n "yoursecret" | kubeseal --controller-namespace sealed-secrets --raw --scope cluster-wide --from-file=/dev/stdin --controller-name sealed-secrets
```

Place these encrypted secrets into values.yaml.


## Add the repo and install Zabbix

```bash
helm repo add alphabet5 https://alphabet5.github.io/helm-charts
helm install zabbix alphabet5/zabbix
```
