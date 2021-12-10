+++
title = "Kubernetes, Kesako ?"
description = "Opinionated explanation of Kubernetes concepts"
date = 2021-12-09T09:19:45+00:00
updated = 2021-12-09T09:19:45+00:00
draft = false
template = "blog/page.html"

[taxonomies]
authors = ["Pierre Baconnier"]
+++

## What is Kubernetes ? (controller-centric opinionated explanation)

**Initial release: 7 June 2014; 7 years ago**

Aujourd'hui, en 2021, force est de constater que Kubernetes est devenu incontournable et la plupart des cloud providers proposent désormais des solutions Kubernetes hébergées.

Il faudra attendre **2013 pour que Google lance sa première offre IAAS** en ayant été ralenti par l'échec de son PAAS AppEngine.

Un des enjeux de K8S fut de réussir a mixer la flexibilité du IAAS et la facilité d'accès, la descriptabilité du PAAS, ce que l'on pourrait appeler l'IPAAS (merci à @Guilhem pour ce terme).

Il fallait donc un standrad pour l'orchestration, une API qui puisse gérer l'ensemble de ces caractéristiques.

Kubernetes est strictement impératif, je demande un état, je me fiche de connaître les étapes intermédiaires.

Par exemple:

- "J'aimerai un espace isolé dont les applications n'utilisent que 4Go de RAM et 2 CPU"
- "J'aimerai une base de données répliquée 2 fois"
- "J'aimerai un storage avec une rétention de 6 mois"
- "J'aimerai que des mises a jour soit déployées mais n'impactent que les applications du ns 'web' ou correspondant au tag 'web'"

## Terminologie

Kubernetes est un système d’orchestration qui permet de déployer et de gérer des conteneurs. Les conteneurs ne sont pas gérés individuellement. Au lieu de cela ils font partie d’un ensemble plus grand appelé **Pod**.

Un Pod se compose d’un ou de plusieurs conteneurs qui partagent une adresse IP, un accès au stockage et un espace de nommage.

**L’orchestration est gérée par des contrôleurs**. Ces contrôleurs sont compilés dans le kube-controller-manager.

Un service est une abstraction qui **définit un ensemble logique de Pods**.

Le ReplicaSet est un contrôleur qui déploie et redémarre les pods. Le ReplicaSet démarre ou arrête des conteneurs. **Il est la pour vérifier que le bon nombre de conteneurs est actif**.

Il y a aussi des **Jobs and CronJobs** pour s’occuper de tâches uniques ou récurrentes.

Gérer facilement des milliers de Pods sur des centaines de nœuds peut s’avérer difficile. 
Pour faciliter la gestion il faut utiliser des labels. Les labels sont des chaînes arbitraires qui font partie des métadonnées de l’objet. Les labels peuvent être utilisés pour changer l’état des objets sans avoir à en connaître les noms individuels ou les UIDs.

## Le state

On va distinguer la notion de spec de la notion de statut.

S'il réussi a converger à l'issue de sa boucle de réconciliation, l'état est appliqué et le résultat retourné à l'api server.

Il était très compliqué d'atteindre l'indempotence avec Puppet ou Chef par exemple, car outre l'effet snowflake, certains composants tel le réseau on-premise, n'étaient pas prévus pour être indompotents.

Deux machines, 2 versions ne vont pas nécessairement réagir de la même façon. C'est l'effet snowflake car tous les flocons se ressemblent et il devient presque impossible distinguer/troubleshooter les différences sur des milliers de machines de production.

Kubernetes répond a ce problème car il maintient l'état et le retourne après action. On a donc un *avant* et un *après*.

Les pratiques de Kube sont basées sur Google mais également sur beaucoup d'autres suggestions de la communauté.

L'organe de certification de la CNCF permet d'assurer que toutes les solutions qui sont vendues sont compatibles.

On a un cluster ETCD qui sert a la persistence de l'état. 

L'API server est en somme un "gardien du temple" qui s'assure de la validité des requêtes émises.


### Une certaine approche des problèmes

A quoi sert d'avoir plusieurs clusters quand on pourrait tout mutualiser sur un cluster/context/tenant ?

Parce que cette centralisation ne permet pas une bonne organisation ni d'être scalable simplement ni de patcher efficacement.

Cela entraînera nécessairement des contraintes sécuritaiers de type enforcement des pod security rules, non-root user ou encore priviledged escalation files par exemple.

On aura donc moins de latitude sur un unique cluster car tout le monde a un besoin différent donc c'est la règle de la contrainte la plus forte va s'appliquer a tous.
