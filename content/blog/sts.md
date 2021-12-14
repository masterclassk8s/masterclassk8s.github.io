+++
title = "Stateful K8s applications"
description = "Introduce K8S stateful applications concepts"
draft = false
date = 2021-12-09T13:49:54+00:00
updated = 2021-12-09T13:49:54+00:00
template = "blog/page.html"

[taxonomies]
authors = ["Pierre Baconnier"]
+++

# Stateful applications in EKS

https://www.nebulaworks.com/insights/posts/leveraging-aws-ebs-for-kubernetes-persistent-volumes/

One such type of persistent volume is the AWSElasticBlockStore which is the type this blog will focus on.

## Steps

```bash
aws ec2 create-volume --availability-zone=eu-west-1a --size=10 --volume-type=gp2

aws ec2 attach-volume --device /dev/xvdf --instance-id <MASTER NODE ID> --volume-id <YOUR VOLUME ID>

sudo mkfs -t xfs /dev/<NAME OF VOLUME FROM PREV STEP>

kubectl apply -f https://raw.githubusercontent.com/kubernetes/website/main/content/en/examples/application/mysql/mysql-deployment.yaml

kubectl apply -f https://raw.githubusercontent.com/kubernetes/website/main/content/en/examples/application/mysql/mysql-pv.yaml

kubectl describe deployment mysql

kubectl run -it --rm --image=mysql:5.6 --restart=Never mysql-client -- mysql -h mysql -ppassword

kubectl delete deployment,svc mysql
kubectl delete pvc mysql-pv-claim
kubectl delete pv mysql-pv-volume
```