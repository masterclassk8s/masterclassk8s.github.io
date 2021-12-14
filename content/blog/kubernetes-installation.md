+++
title = "Kubernetes Installation"
description = "Easy or less easy ways to spawn and configure kubernetes cluster"
date = 2021-12-09T09:19:42+00:00
updated = 2021-12-09T09:19:42+00:00
draft = false
template = "blog/page.html"

[taxonomies]
authors = ["Pierre Baconnier"]
+++

## Installation with kubeadm

[Create cluster from scratch with kubeadm](https://v1-18.docs.kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/)

## Installation on AWS via kops

[Cluster install on AWS with KOPS](https://v1-18.docs.kubernetes.io/docs/setup/production-environment/tools/kops/)

[Classic way cluster install on AWS with Turnkey](https://v1-18.docs.kubernetes.io/docs/setup/production-environment/turnkey/aws/)

## Kubernetes on openstack

[Installing Kubernetes on OpenStack](https://superuser.openstack.org/articles/run-your-kubernetes-cluster-on-openstack-in-production/)

[Installing Kubernetes on OpenStack and Bare-metal](https://www.openstack.org/videos/summits/berlin-2018/running-kubernetes-on-openstack-and-bare-metal)

[Installing Kubernetes on OpenStack and AWS the hard way](https://automationlogic.com/installing-open-stack-on-aws-by-george-tarnaras/)

## After POC on AWS...

### The hard way, Kops

```bash
export AWS_ACCESS_KEY_ID=""
export AWS_SECRET_ACCESS_KEY=""
export AWS_DEFAULT_REGION=""

aws sts get-caller-identity

curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64

chmod +x kops-linux-amd64

sudo mv kops-linux-amd64 /usr/local/bin/kops

aws route53 create-hosted-zone --name kolorado.e-monkeys.tech --caller-reference 10

dig NS kolorado.e-monkeys.tech

aws s3 mb s3://kolorado.e-monkeys.tech

export KOPS_STATE_STORE=s3://kolorado.e-monkeys.tech

kops create cluster kolorado.e-monkeys.tech --cloud aws --zones us-east-2c
kops get cluster
kops edit cluster kolorado.e-monkeys.tech
kops edit ig --name=kolorado.e-monkeys.tech nodes
kops edit ig --name=kolorado.e-monkeys.tech master-us-east
kops update cluster --name kolorado.e-monkeys.tech --yes --admin
kops rolling-update cluster --cloudonly

# get kubeconfig in s3://kolorado.e-monkeys.tech
Using cluster from kubectl context: kolorado.dev.e-monkeys.tech

NAME                    STATUS  NEEDUPDATE      READY   MIN     TARGET  MAX
master-us-east-2c       Ready   0               1       1       1       1
nodes-us-east-2c        Ready   0               1       1       1       1

kops validate cluster --wait 10m

kubectl get nodes --show-labels
```

## Delete cluster with Kops

```bash
kops delete cluster kolorado.e-monkeys.tech --yes
```

### The easy/good K8s on AWS way, eksctl

Spawning nodes mode of eksctl:

- Fargate - Linux - Sélectionnez ce type de nœud si vous souhaitez exécuter des applications Linux sur AWS Fargate. Fargate est un moteur de calcul sans serveur qui vous permet de déployer des pods Kubernetes sans gérer les instances Amazon EC2.

- Nœuds gérés - Linux : sélectionnez ce type de nœud si vous souhaitez exécuter des applications Amazon Linux sur des instances Amazon EC2. Bien que cela ne soit pas abordé dans ce guide, vous pouvez également ajouter des nœuds autogérés et Bottlerocket Windows à votre cluster.

```bash
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin

02:34:48 pbackz@LAPTOP-UJNQHICP ~ → eksctl create cluster --name kolorado --region us-east-2 --fargate
2021-12-14 02:35:13 [ℹ]  eksctl version 0.76.0
2021-12-14 02:35:13 [ℹ]  using region us-east-2
2021-12-14 02:35:13 [ℹ]  setting availability zones to [us-east-2a us-east-2b us-east-2c]
2021-12-14 02:35:13 [ℹ]  subnets for us-east-2a - public:192.168.0.0/19 private:192.168.96.0/19
2021-12-14 02:35:13 [ℹ]  subnets for us-east-2b - public:192.168.32.0/19 private:192.168.128.0/19
2021-12-14 02:35:13 [ℹ]  subnets for us-east-2c - public:192.168.64.0/19 private:192.168.160.0/19
2021-12-14 02:35:13 [ℹ]  using Kubernetes version 1.21
2021-12-14 02:35:13 [ℹ]  creating EKS cluster "kolorado" in "us-east-2" region with Fargate profile
2021-12-14 02:35:13 [ℹ]  if you encounter any issues, check CloudFormation console or try 'eksctl utils describe-stacks --region=us-east-2 --cluster=kolorado'
2021-12-14 02:35:13 [ℹ]  CloudWatch logging will not be enabled for cluster "kolorado" in "us-east-2"
2021-12-14 02:35:13 [ℹ]  you can enable it with 'eksctl utils update-cluster-logging --enable-types={SPECIFY-YOUR-LOG-TYPES-HERE (e.g. all)} --region=us-east-2 --cluster=kolorado'
2021-12-14 02:35:13 [ℹ]  Kubernetes API endpoint access will use default of {publicAccess=true, privateAccess=false} for cluster "kolorado" in "us-east-2"
2021-12-14 02:35:13 [ℹ]
2 sequential tasks: { create cluster control plane "kolorado",
    2 sequential sub-tasks: {
        wait for control plane to become ready,
        create fargate profiles,
    }
}
2021-12-14 02:35:13 [ℹ]  building cluster stack "eksctl-kolorado-cluster"
2021-12-14 02:35:14 [ℹ]  deploying stack "eksctl-kolorado-cluster"
2021-12-14 02:35:44 [ℹ]  waiting for CloudFormation stack "eksctl-kolorado-cluster"
2021-12-14 02:36:14 [ℹ]  waiting for CloudFormation stack "eksctl-kolorado-cluster"
2021-12-14 02:37:15 [ℹ]  waiting for CloudFormation stack "eksctl-kolorado-cluster"
2021-12-14 02:38:15 [ℹ]  waiting for CloudFormation stack "eksctl-kolorado-cluster"
2021-12-14 02:39:16 [ℹ]  waiting for CloudFormation stack "eksctl-kolorado-cluster"
2021-12-14 02:40:16 [ℹ]  waiting for CloudFormation stack "eksctl-kolorado-cluster"
2021-12-14 02:41:16 [ℹ]  waiting for CloudFormation stack "eksctl-kolorado-cluster"
2021-12-14 02:42:17 [ℹ]  waiting for CloudFormation stack "eksctl-kolorado-cluster"


2021-12-14 02:43:17 [ℹ]  waiting for CloudFormation stack "eksctl-kolorado-cluster"
2021-12-14 02:44:18 [ℹ]  waiting for CloudFormation stack "eksctl-kolorado-cluster"
2021-12-14 02:45:18 [ℹ]  waiting for CloudFormation stack "eksctl-kolorado-cluster"
2021-12-14 02:46:19 [ℹ]  waiting for CloudFormation stack "eksctl-kolorado-cluster"
2021-12-14 02:47:19 [ℹ]  waiting for CloudFormation stack "eksctl-kolorado-cluster"
2021-12-14 02:48:19 [ℹ]  waiting for CloudFormation stack "eksctl-kolorado-cluster"
2021-12-14 02:50:22 [ℹ]  creating Fargate profile "fp-default" on EKS cluster "kolorado"

2021-12-14 02:54:41 [ℹ]  created Fargate profile "fp-default" on EKS cluster "kolorado"
2021-12-14 02:57:13 [ℹ]  "coredns" is now schedulable onto Fargate
2021-12-14 02:58:17 [ℹ]  "coredns" is now scheduled onto Fargate
2021-12-14 02:58:17 [ℹ]  "coredns" pods are now scheduled onto Fargate
2021-12-14 02:58:17 [ℹ]  waiting for the control plane availability...
2021-12-14 02:58:17 [✔]  saved kubeconfig as "/home/pbackz/.kube/config"
2021-12-14 02:58:17 [ℹ]  no tasks
2021-12-14 02:58:17 [✔]  all EKS cluster resources for "kolorado" have been created
2021-12-14 02:58:19 [ℹ]  kubectl command should work with "/home/pbackz/.kube/config", try 'kubectl get nodes'
2021-12-14 02:58:19 [✔]  EKS cluster "kolorado" in "us-east-2" region is ready

# Validate installation
kubectl get pods --all-namespaces -o wide

# Delete cluster
eksctl delete cluster --name my-cluster --region region-code

# Show config and hacks cli
alias k='kubectl'
k config view
```

```yaml
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://D574D5657EED6B65990285B60D420EBB.gr7.us-east-2.eks.amazonaws.com
  name: kolorado.us-east-2.eksctl.io
contexts:
- context:
    cluster: kolorado.us-east-2.eksctl.io
    user: kolo@kolorado.us-east-2.eksctl.io
  name: kolo@kolorado.us-east-2.eksctl.io
current-context: kolo@kolorado.us-east-2.eksctl.io
kind: Config
preferences: {}
users:
- name: kolo@kolorado.us-east-2.eksctl.io
  user:
    exec:
      apiVersion: client.authentication.k8s.io/v1alpha1
      args:
      - eks
      - get-token
      - --cluster-name
      - kolorado
      - --region
      - us-east-2
      command: aws
      env:
      - name: AWS_STS_REGIONAL_ENDPOINTS
        value: regional
      interactiveMode: IfAvailable
      provideClusterInfo: false
```

https://github.com/weaveworks/eksctl/tree/main/examples
