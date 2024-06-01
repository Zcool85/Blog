---
title:  "Circuit boost 5V depuis des accu LR06"
type: Fun
last_modified_at: 2023-02-19 21:55:00 +0100
state: Terminé
---

Petit circuit permettant d'obtenir une tension régulée de 5V / 1A à partir de 3 piles/accus LR06.

<!--more-->

Dépôt GitHub : [https://github.com/Zcool85/Batteries-3v-5v-StepUp](https://github.com/Zcool85/Batteries-3v-5v-StepUp){:target="_blank"}

Mes contraintes :
- Usage de piles LR06 ou d'ACCU LR06
- Pouvoir débiter 1A sous 5V en sortie
- Le circuit doit tenir dans un petit boitier
- J'ai décidé d'utiliser le circuit MAX641 qui permet de faire une régulation boost de 5V avec peu de composants autour

## Shémas de base

Ce circuit peut être utilisé directement en mode "Low power" limité à 50mA. La datasheet du composant montre la circuiteriez des base dans ce cas :

![Schéma Low Power](/assets/projects/BatteriesStepUp/low-power-schematics.png){: width="400" }
_En mode Low Power_

Le second mode de fonctionnement possible est en mode "High output current" (1A) en ajoutant un transistor et une diode externe au montage. Là encore, la datasheet nous montre le montage de base correspondant :

![High output current](/assets/projects/BatteriesStepUp/high-output-current-schematics.png){: width="400" }
_En mode High output current_


## PCB

Côté PCB, j'ai opté pour la taille la plus grande que je pouvais avoir pour le boitier que j'ai choisi :


![PCB](/assets/projects/BatteriesStepUp/PCB.png){: width="400" }
_PCB_


## Réalisation

Après commande du PCB sur https://jlcpcb.com et un peu de travaux mécaniques, j'en arrive à ça :

![Intérieur](/assets/projects/BatteriesStepUp/internal.jpeg){: width="400" }
_Intérieur_

![Panneau avant](/assets/projects/BatteriesStepUp/front-panel.jpeg){: width="400" }
_Panneau avant_
