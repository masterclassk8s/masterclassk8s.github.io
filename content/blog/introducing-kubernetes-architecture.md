+++
title = "Introducing Kubernetes Architecture"
description = "Article to introduce basic concepts of Kubernetes Architecture and detailed marketplace solutions"
draft = false
date = 2021-12-09T13:49:54+00:00
updated = 2021-12-09T13:49:54+00:00
template = "blog/page.html"

[taxonomies]
authors = ["Pierre Baconnier"]
+++

## Kubernetes Architecture

### Component layers architecture

<img src="https://masterclassk8s.github.io/blog/control-plane.png" alt="Simple_Control_Plane_Schema"> 


### Macro architecture 

<img src="https://masterclassK8s.github.io/blog/Kubernetes-101-Architecture-Diagram.jpg" alt="Other_Control_Plane_Schema"> 


## CNCF Landscape

<img src="https://masterclassK8s.github.io/blog/cncf-landscape.jpg" alt="CNCF_Landscape" width="1100" height="1000"> 


## First cluster: Show cluster info

```bash
controlplane $ kubectl cluster-info

Kubernetes master is running at https://172.17.0.10:6443
KubeDNS is running at https://172.17.0.10:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```
