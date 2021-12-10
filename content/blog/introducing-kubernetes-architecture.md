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


## Kubernetes Solutions

### K8S certified providers

[Marketplace Solutions](https://v1-18.docs.kubernetes.io/fr/docs/setup/pick-right-solution)

#### Managed Solutions

#### Le cas OVH: implémentation d'offre KAAS basée sur OpenStack et Platform9 solution

Dans le cadre de la Kubecon 2019, OVH annonce le déploiement de Kubernetes sur ses serveurs dédiés suite à un partenariat avec **Platform9**. Par conséquent, il est désormais le fournisseur européen qui propose le choix le plus diversifié en matière de déploiement Kubernetes.

#### Le cas Bare-metal

En bare-metal l'expérience montre qu'avec CoreOs racheté par Redhat et intégré a Openshift, cette solution ainsi que Pivotal et Rancher sont les plus représentées sur le marché français.

## CNCF

### CNCF Landscape

<img src="https://masterclassK8s.github.io/blog/cncf-landscape.jpg" alt="CNCF_Landscape" width="940" height="800"> 

### Certification

[https://training.linuxfoundation.org/training/kubernetes-fundamentals/](https://training.linuxfoundation.org/training/kubernetes-fundamentals/)


## Control Plane components

> At least, Kubernetes worker nodes must be owned 3 system prcocessus and 1 container runtime 

Le Control Plane est constitué des éléments suivants:

- cluster etcd: 
> Un stockage simple et distribué de valeurs de clés qui est utilisé pour stocker les données du cluster Kubernetes (telles que le nombre de pods, leur état, l'espace de noms, etc), les objets API et les détails de la découverte de services. Pour des raisons de sécurité, il n'est accessible qu'à partir du serveur d'API. etc. etcd permet de notifier au cluster les changements de configuration à l'aide de surveillants. Les notifications sont des requêtes API sur chaque nœud du cluster etcd pour déclencher la mise à jour des informations dans le stockage du nœud.


- kube-apiserver:
> Le serveur API Kubernetes est l'entité de gestion centrale qui reçoit toutes les demandes REST de modifications (aux pods, services, ensembles de réplication/contrôleurs et autres), servant de front-end au cluster. C'est également le seul composant qui communique avec le cluster etcd, s'assurant que les données sont stockées dans etcd et sont en accord avec les détails des services des pods déployés.


- kube-controller-manager:
> Exécute un certain nombre de processus de contrôle distincts en arrière-plan (par exemple, le contrôleur de réplication contrôle le nombre de replica dans un pod, le contrôleur de endpoints remplit les objets endpints comme les services et les pods) pour réguler l'état partagé du cluster et effectuer des tâches de routine. Lorsqu'un changement dans la configuration d'un service se produit (par exemple, le remplacement de l'image à partir de laquelle les pods sont exécutés, ou la modification des paramètres dans le fichier yaml de configuration), le contrôleur repère le changement et commence à travailler vers le nouvel état souhaité.


- cloud-controller-manager:
> Le responsable de la gestion des processus du contrôleur qui dépendent du cloud provider sous-jacent (le cas échéant). Par exemple, lorsqu'un contrôleur doit vérifier si un nœud a été résilié ou configurer des routes, des équilibreurs de charge ou des volumes dans l'infrastructure du cloud provider, tout cela est géré par le cloud provider.


- kube-scheduler:
> Aide à planifier les pods (un groupe de conteneurs co-localisés à l'intérieur desquels nos processus d'application sont exécutés) sur les différents nœuds en fonction de l'utilisation des ressources. Il lit les exigences opérationnelles du service et le planifie sur le nœud le mieux adapté. Par exemple, si l'application a besoin de 1 Go de mémoire et de 2 cœurs CPU, les pods de cette application seront planifiés sur un nœud disposant au moins de ces ressources. Le planificateur s'exécute chaque fois qu'il est nécessaire de planifier des pods. Le planificateur doit connaître les ressources totales disponibles ainsi que les ressources allouées aux charges de travail existantes sur chaque nœud.
