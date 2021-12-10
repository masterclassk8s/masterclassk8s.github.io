+++
title = "Containers Kesacko"
description = "Opinionated explanation of Linux Containers concepts"
date = 2021-04-20T09:19:42+00:00
updated = 2021-04-20T09:19:42+00:00
draft = false
template = "blog/page.html"

[taxonomies]
authors = ["Pierre Baconnier"]
+++

## What is a container

> "Caisson métallique parallélépipédique conçu pour le transport de marchandise par différents mode de transports.
> Ses dimensions ont été standardisées au niveau international et il est muni a tous les angles de pièces de préemption
> permettant de l'arrimer ou de le transborder d'un véhicule a l'autre." 
*Définition d'un container maritime*

C'est tout simplement faire tourner un ensemble de processus systèmes ou métier au sein d'un même environnement, isolé.

Une des problématique de la virtualisation par hyperviseur est la problématique de la boîte noire. Difficile de recenser l'ensemble des composants, ce qui peut être critique en cas de panne de la VM, impossible donc d'en déterminer l'état.

Comment faire également lorsque les ressources ne sont pas partagées, comme le cache des applications par exemple ? 

Physiquement, un container est un système de fichiers contenant l'ensemble des binaires dont ont a besoin pour faire tourner
notre application.

L'image apporte également des métadonnées tels des layers dont ont peut déterminer la composition exacte.

A la fin on a une brique atomique, je n'ai pas besoin de savoir quelles sont les couches successivement empilées du container.

Cela permet de résoudre la problématique du packaging OS (.deb, .rpm, etc.), non agnostique.

Les containers sont immutables, ils ne laissent rien derrière eux, ont un format autosufisant.

Les développeurs peuvent passer d'un langage a l'autre pour les applications sans jamais impacter l'infrastructure de PROD.

C'est toujours un 'runner' de container tel containerd qui execute le processus sur l'hôte mais on se fiche désormais de savoir quel est le launcher sous le container (npm, shell, cargo, etc.).

Les containers amènent de l'auditabilité en production.

Autour de cette infrastructure, d'autres composants, tel un orchestrateur peuvent être intégrés.

C'est dans cette optique que nous pouvons désormais sereinement introduire les concepts Kubernetes.

### Quelques infos en vrac

Exemple de toolbox minimaliste: [Busybox](https://www.busybox.net)

Outil d'introspection des layers des images: [Dive](https://github.com/wagoodman/dive)

Container runtime de docker: [Containerd](https://containerd.ios)

Docker multistage building: [Multistage building](https://docs.docker.com/develop/develop-images/multistage-build)