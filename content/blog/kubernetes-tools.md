+++
title = "Kubernetes Tooling"
description = "Client tools to mastering Kubernetes usage and increases productivity"
date = 2021-12-09T09:19:56+00:00
updated = 2021-12-09T09:19:56+00:00
draft = false
template = "blog/page.html"

[taxonomies]
authors = ["Pierre Baconnier"]
+++

# Kubernetes Client Tools

## Krew: plugin manager for Kubernetes

```bash
(
  set -x; cd "$(mktemp -d)" &&
  OS="$(uname | tr '[:upper:]' '[:lower:]')" &&
  ARCH="$(uname -m | sed -e 's/x86_64/amd64/' -e 's/\(arm\)\(64\)\?.*/\1\2/' -e 's/aarch64$/arm64/')" &&
  KREW="krew-${OS}_${ARCH}" &&
  curl -fsSLO "https://github.com/kubernetes-sigs/krew/releases/latest/download/${KREW}.tar.gz" &&
  tar zxvf "${KREW}.tar.gz" &&
  ./"${KREW}" install krew
)

echo "export PATH='${KREW_ROOT:-$HOME/.krew}/bin:$PATH'" >> ~/.bashrc
```

## Kubectx + Kubens

```bash
kubectl krew install ctx
kubectl krew install ns
```

## Helm

```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

### Helm on Windows

```pwsh
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

```pwsh
choco install kubernetes-helm
```

## Kubie

## K9s

