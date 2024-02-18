---
title:  "Détecteur électrostatique"
type: Fun
last_modified_at: 2024-02-18 21:40:00 +0100
state: Terminé
---

Un tout petit projet permettant de détecter des champs électrostatique.

<!--more-->

Dépôt GitHub : [https://github.com/Zcool85/ElectrostaticDetector](https://github.com/Zcool85/ElectrostaticDetector){:target="_blank"}


## Schéma et principe de fonctionnement

![Schéma du détecteur](/assets/projects/ElectrostaticDetector/Schematic.png)
_Schéma de principe du détecteur électrostatique_

Le principe de fonctionnement de ce schéma est plutôt simple :

Il y a deux circuits indépendant avec chacun des mosfets. Chaque mosfet bloque l'alumage d'une LED.

Le canal N bloque la cathode de la LED rouge; Le canal P bloque l'anode de la LED bleu.

Les antennes sont reliées à chacune des grilles des mosfets.

L'application d'une tension (dans notre cas, une tension électrostatique) entre la grille et la source du mosfet va rendre passant le courant entre le drain et la source alumant ainsi la LED.

Pour un canal N, la tension Vgs doit être positive pour qu'il entre en conduction. Pour un canal P, la tension Vgs doit être négative.


## Implémentation du projet

Le montage n'étant pas très compliqué (et pour faire vite), j'ai tout implémenté sur plaque à souder d'expérimentation.

![Montage terminé](/assets/projects/ElectrostaticDetector/IMG_6537.jpeg)
_Photo du montage terminé_

![Avec les antennes](/assets/projects/ElectrostaticDetector/IMG_6538.jpeg)
_Photo avec les antennes_

![Vidéo](/assets/projects/ElectrostaticDetector/IMG_6536.MOV)
_Vidéo en fonctionnement_
