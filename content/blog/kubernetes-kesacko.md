+++
title = "Kubernetes Kesacko"
description = "Opinionated explanation of Kubernetes concepts"
date = 2021-12-09T09:19:45+00:00
updated = 2021-12-09T09:19:45+00:00
draft = false
template = "blog/page.html"

[taxonomies]
authors = ["Pierre Baconnier"]
+++

## What is Kubernetes (Controller-centric opinionated explanation)

**Initial release: 7 June 2014; 7 years ago**

Aujourd'hui, en 2021, Kubernetes est devenu incontournable et la plupart des cloud providers proposent désormais des solutions Kubernetes hébergées.

Il faudra attendre **2013 pour que Google lance sa première offre IAAS** en ayant été ralenti par l'échec de son PAAS AppEngine.

Un des enjeux de K8S fut de réussir a mixer la flexibilité du IAAS et la facilité d'accès, la descriptabilité du PAAS, ce que l'on pourrait appeler l'IPAAS (merci à @Guilhem pour ce terme).

Il fallait donc un standrad pour l'orchestration, une API qui puisse gérer l'ensemble de ces caractéristiques.

Kubernetes est strictement impératif, je demande un état, je me fiche de connaître les étapes intermédiaires.

Par exemple:

- "J'aimerai un espace isolé dont les applications n'utilisent que 4Go de RAM et 2 CPU"
- "J'aimerai une base de données répliquée 2 fois"
- "J'aimerai un storage avec une rétention de 6 mois"
- "J'aimerai que des mises a jour soit déployées mais n'impactent que les applications du ns 'web' ou correspondant au tag 'web'"

On va distinguer la notion de spec de la notion de statut.

S'il réussi a converger à l'issue de sa boucle de réconciliation, l'état est appliqué et le résultat retourné à l'api server.

Il était très compliqué d'atteindre l'indempotence avec Puppet ou Chef par exemple, car outre l'effet snowflake, certains composants tel le réseau on-premise, n'étaient pas prévus pour être indompotents.

Deux machines, 2 versions ne vont pas nécessairement réagir de la même façon, c'est l'effet snowflake car tous les flocons se ressemblent et c'est très compliqué a distinguer/troubleshooter sur des milliers de machines de production.

Kubernetes répond a ce problème car il maintient l'état le retourne après action. On a donc un "avant" et un "après".

Les pratiques de Kube sont basées sur Google mais également sur beaucoup d'autres suggestions de la communauté.

L'organe de certification de la CNCF permet d'assurer que toutes les solutions qui sont vendues sont compatibles.

On a un cluster ETCD qui sert a la persistence de l'état. L'API server est une "sorte de gardien du temple".

### Une certaine approche des problèmes

A quoi sert d'avoir plusieurs clusters quand on pourrait tout mutualiser sur un cluster/context/tenant ?

Parce que cette centralisation ne permet pas une bonne organisation ni d'être scalable simplement ni de patcher efficacement.

Cela entraînera nécessairement des contraintes sécuritaiers de type enforcement des pod security rules, non-root user ou encore priviledged escalation files par exemple.

Moins de latitude sur un unique cluster car tout le monde a un besoin différent donc c'est la règle de la contrainte la plus forte va s'appliquer a tous.

## Kubernetes Solutions

### K8S certified providers

[Marketplace Solutions](https://v1-18.docs.kubernetes.io/fr/docs/setup/pick-right-solution)

#### Le cas OVH: implémentation d'offre KAAS basée sur OpenStack et Platform9 solution

Dans le cadre de la Kubecon 2019, OVH annonce le déploiement de Kubernetes sur ses serveurs dédiés suite à un partenariat avec **Platform9**. Par conséquent, il est désormais le fournisseur européen qui propose le choix le plus diversifié en matière de déploiement Kubernetes.

### Bare-metal

En bare-metal l'expérience montre qu'avec CoreOs racheté par Redhat et intégré a Openshift, cette solution ainsi que Pivotal et Rancher sont les plus représentées sur le marché français.

## CNCF

### Certification

https://training.linuxfoundation.org/training/kubernetes-fundamentals/