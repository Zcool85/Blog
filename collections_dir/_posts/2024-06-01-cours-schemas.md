---
layout: post
author: zcool
title:  "Réaliser des schémas électroniques"
categories: [Electronique, Cours]
---

Cette page est consacrée à la réalisation des schémas électroniques. Il faut savoir qu'en fait, les schémas électroniques suivent des conventions et non des normes. Cela signifie qu'un même schéma a des représentation souvent différentes d'une personne à l'autre. De ce fait, je me focaliserais principalement sur les conventions que j'utilise dans mes différents schémas.

Nous allons tout d'abord voir le schéma des différents composants que j'utilise en essayant d'expliquer à quoi le dit-composant peut servir. Puis, dans une seconde partie, je donnerais quelques "astuces" sur la création des schémas.

## Schéma de composants "classiques"

![Alimentation alternative](/assets/posts/learning-schematics/vac.png)
_Alimentation alternative_

![Alimentation continue](/assets/posts/learning-schematics/vcc.png)
_Alimentation continue_

![Batterie](/assets/posts/learning-schematics/batt.png)
_Batterie_

![Interrupteur à bascule](/assets/posts/learning-schematics/switch.png)
_Interrupteur à bascule_

![LED](/assets/posts/learning-schematics/led.png)
_LED_

![Résistance](/assets/posts/learning-schematics/res.png)
_Résistance_


## Trucs et astuces

### Logiciels de schématiques

Personnellement, j'utilise le logiciel [KiCad](https://www.kicad.org). Ce dernier est en fait une suite de logiciels permettant de réaliser des schémas électronique jusqu'à la réalisation des PCB (circuits imprimés). Ce logiciel est gratuit et OpenSource.


### Les conventions de nommages

Dans un schéma électronique, chaque composant est nommé par un code qui est en général d'une à deux lettres par type de composant suivi par un numéro chrono. C'est à partir de ces codes que les listes de composants (ou netlists) sont créées par les logiciels ou dans les revues spécialisées. C'est un moyen pratique pour ne pas encombrer l'espace dans le schéma et déporter les caractéristiques techniques de chaque composant dans une liste séparée.

Par exemple, le schéma suivant montre un montage classique pour allumer une diode électroluminescente :

![Exemple de schéma](/assets/posts/learning-schematics/sample.png)
_Exemple de schéma_

Voici la liste des composants associés :

- BT1 : Batterie 9V
- SW1 : Interrupteur à bascule On-Off
- R1 : Résistance 1/4 Watt 1500 Ohms
- D1 : Diode électroluminescente verte 1,2 Volts à 5mA
