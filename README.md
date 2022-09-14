# ArgoCD app of apps pattern setup

<img src="https://argo-cd.readthedocs.io/en/stable/assets/logo.png">

## What is this project

This repo aims to serve as a starting point for anyone interested in setting up [app of apps pattern](https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/#app-of-apps-pattern) in Argo.

👉 Feel free to open an issue if you encounter a problem.

### Set up

Assuming you already have a cluster follow the commands below.
To test locally use something like [rancher desktop](https://rancherdesktop.io)
If you want to spawn a cluster fast open the toggle

<details>
<summary>Quickly spawn a cluster if you don't have one</summary>

SSH in a node and install k3s

```
curl -sfL https://get.k3s.io | sh -s - --node-name k3s-master-01
```

Get token so that you can connect other nodes

```
cat /var/lib/rancher/k3s/server/node-token
```

Copy kubeconfig so you can run commands from your machine

```
cat /etc/rancher/k3s/k3s.yaml
```

</details>

### Configure the files

1. Open your text editor
1. Perform a global search on: `<Change me>` and change all occurrences with your data. It should be easy. There are comments next to all the properties.
1. You may want to remove `kubeconfig.yaml` from version control.

### Install

Follow the install instructions below by running each command.

👉 Create a namespace for argo to be installed in

```
kubectl --kubeconfig kubeconfig.yaml create namespace argocd
```

👉 Install argo

```
kubectl apply --kubeconfig kubeconfig.yaml -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

kubectl apply --kubeconfig kubeconfig.yaml -k argocd
```

👉 Get secret so you can login to Argo UI

```
kubectl -n argocd get --kubeconfig kubeconfig.yaml secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

You can now port forward form your favorite tool to argo and log in to the UI.

### Optional: Configure a domain to point to argo.

Since there is already an ingress controller you can just point a domain to your IP directly. An easy way is to use cloudflare which will also manage your TLS certs for you.

## Todos

- [ ] Finish with documentation in detail
- [ ] Optional: Create a cli to change values
