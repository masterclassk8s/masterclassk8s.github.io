+++
title = "Kubernetes basics"
description = "Basic concepts to communicate with K8S api server"
date = 2021-12-09T09:19:50+00:00
updated = 2021-12-09T09:19:50+00:00
draft = false
template = "blog/page.html"

[taxonomies]
authors = ["Pierre Baconnier"]
+++

## Kubernetes basics

### Testing first cluster: Show cluster info

```bash
controlplane $ kubectl cluster-info

Kubernetes master is running at https://172.17.0.10:6443
KubeDNS is running at https://172.17.0.10:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```

### Deploying in Kube

Comment on spawn un container dans Kube ?
C'est décrire l'état final qu'on veut dans Kubernetes.
Kubernetes tentera de converger vers cet état lors de sa boucle de réconciliation.

On lui demande le nombre de replica que l'on souhaite poi=ur une même ressource.

- 1st case: deploying in specific limited namespace

- 2nd case: 

### Deployment tools

- CD (PAAS over Kube)

- Helm

- Helmfile

