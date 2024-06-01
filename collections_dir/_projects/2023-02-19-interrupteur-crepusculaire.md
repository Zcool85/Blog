---
title:  "Interrupteur crépusculaire"
type: Fun
last_modified_at: 2023-02-19 21:55:00 +0100
state: Terminé
---

Circuit très simple d'interrupteur crépusculaire.

<!--more-->

Dépôt GitHub : [https://github.com/Zcool85/Twilight-Switch](https://github.com/Zcool85/Twilight-Switch){:target="_blank"}

J'ai conçu ce projet pour mon fils pour lui faire découvrir quelques possibilités techniques avec l'électronique.

L'objectif de ce projet est de réaliser un petit montage qui allume une LED lorsque la lumière est trop basse.

## Shémas

![Schéma](/assets/projects/Twilight-Switch/schematics.png){: width="400" }
_Schéma_

N'importe quel transistor bipolaire NPN peut faire l'affaire.

Le fonctionnement synthétique est le suivant :
- Lorsque la luminosité est forte, R1 a une résistance faible. Donc la tension sur TP1 est supérieure à 0,6V. Le transistor Q1 conduit et donc Q2 ne conduit pas => La LED D1 est donc éteinte
- A contrario, lorsque la luminosité est faible, R1 a une forte résistance. La tension sur TP1 est donc inférieure à 0,6V. Le transistor Q1 ne conduit pas, c'est donc Q2 qui conduit => La LED D1 est allumée.

Globalement, l'objectif est de trouver la bonne valeur de résistance R2 en fonction de la LDR pour déterminer le seuil de luminosité à partir duquel la LED doit s'allumer. Pour ce circuit, j'ai déterminé les valeurs de manière empirique.
