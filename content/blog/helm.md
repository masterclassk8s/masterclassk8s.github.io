+++
title = "Helm"
description = "Introduce Helm (K8S package manager)"
draft = false
date = 2021-12-09T13:49:54+00:00
updated = 2021-12-09T13:49:54+00:00
template = "blog/page.html"

[taxonomies]
authors = ["Pierre Baconnier"]
+++

## Testing helm-chart (swaggerui)

```bash
helm repo add cetic https://cetic.github.io/helm-charts
helm repo update
helm install --name my-release cetic/swaggerui
```