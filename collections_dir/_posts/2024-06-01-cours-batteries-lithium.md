---
layout: post
author: zcool
title:  "Les batteries au lithium"
categories: [Electronique, Cours]
math: true
---

Nous ne parlerons ici que des batteries de types lithium (lithion/ion ou lithium/polymère).

Une batterie est un assemblage de modules eux-mêmes constitués de cellules.

Trois formats de cellule lithium ion :
- Prismatique
- En pochette
- Cylindrique

Caractéristiques d'une cellule :
- Tension nominal en Volts ($V_{nom}$)
- Capacité nominale en Ampère Heure ($Q$)
- Courant max qu'elle peut fournir en Ampère ($I_{max}$)

La tension varie en fonction de l'état de charge de la cellule entre $V_{min}$ et $V_{max}$.

Un module est un assemblage de cellules en série et en parralèle. On dénomme les modules par "XsYp" pour X cellules en séries et Y en parallèle.

Exemple : Une batterie 4s6p contient 24 cellules au total avec 6 blocs montés en parallèle, chaque bloc contenant 4 cellules montées en série.

![Batterie 4s6p](/assets/posts/learning-batt-lithium/4s6p.png){: width="400" }
_Circuit d'une batterie 4s6p_

Lorsque les cellules sont montées en parallèle :
- On branche leurs bornes + ensemble et leurs bornes - ensemble
- La tension entre les bornes de la batterie est la même qu'une cellule unique
- La capacité de la batterie est la sommes des capacités des cellules
- Le courant de la batterie est la sommes des courants des cellules

Lorsque les cellules sont montées en série :
- On branche le + d'une cellule au - de la suivante, etc.
- La tension entre les bornes est la sommes de toutes les tensions des cellules
- La capacité de la batterie est la même qu'une cellule unique
- Le courant de la batterie est le même qu'une cellule unique

Ces règles s'appliquent aussi pour le chargement des batteries :
- La tension de charge s'aditionne lorsque les batteries sont en séries
- Le courant de charge s'aditionne lorsque les batteries sont en parallèle

> Les tensions $V_{min}$ et $V_{max}$ vont également varier en fonction de la constitution de la batterie.
>
> Exemple pour une cellule dont $V_{min} = 2.5V$ et $V_{max} = 4.2V$, alors pour une 4s6p :
> $V_{min} = 4 \times 2.5 = 10V$
> $V_{max} = 4 \times 4.2 = 16.8V$
{: .prompt-warning }

> Pour vérifier que les tensions soient toujours cohérentes, il est indispensable d'utiliser un BMS (Battery Management System) dès lors que l'on a une batterie lithium.
{: .prompt-danger }


## Dimensionner et réaliser une batterie au lithium ion

> Uniquement lithium ion
{: .prompt-warning }

Cheminement pour fabriquer sa propre batterie :

1. Déterminer l'usage.
    Exemple : Utiliser un moteur $1\,000W$ en $48V$ alimenté par un controlleur sous $15A$ nominal et $30A$ en crète.
    On voudrait une capacité de $400Wh$

2. Déterminer le nombre de modules
    La tension d'une cellule est d'environ $3.6V$; On détermine donc qu'il faudra $\frac{48V}{3.6V} \approx 13.333$.

    Avec 13 modules ayant chacun $V_{min} = 2.5V$ et $V_{max} = 4.2V$, alors la tension du module sera entre $32.5V$ et $54.6V$ (avec $13 \times 3.6V = 46.8V$). C'est correcte, donc on part sur 13s.
    
    A ce stade, avec un seul module (13s1p), l'énergie délivrée est de $V_{bat} \times Q_{bat} = 46.8V \times 2.5 Ah = 117 Wh$.

    On est loin des capacités désirées. De plus, il faudrait que chaque cellule puisse fournir $15A$ nominal et $30A$ en crète. Il y aura donc plusieurs modules en parallèle.

3. Rechercher les bonnes cellules
    Le principe est de trouver les bonnes cellules a acheter en recherchant les meilleurs combinaisons (Décortiquer les datasheets).
    
    Le mieux ici est de faire un petit tableau comparatif en 13s2p vs 13s3p pour voir quelle est la combinaison la plus adapté au projet (emcombrement, capacité, prix, ...).

    Pour l'exemple, nous prendrons une batterie 13s3p.

4. Choisir le BMS
    Le BMS doit avoir comme caractéristiques : Li-ion 13s $48V$ qui peut supporter un courant de décharge d'au moins $30A$ et un courant de charge ??? 8A ???.

    On va chercher dans la datasheet le courant et la tension de charge. Dans notre exemple, si une cellule à un courant de charge de $1.925A$ ($3A$ max) sous $4.25V$ max, alors :
      - le courant de charge de la batterie vaut $I_{ch} = 1.925A \times 3 = 5.775 A$ (car 3p). Et courant de charge maximum $I_{chm} = 3 \times 3 = 9A$.
      - La tension de charge $V_{ch} = 4.25V \times 13 = 55.25V$ max (car 13s)
    
    Il faut donc trouver un chargeur qui a ces caractgéristiques (Exemple $4A$ sous $54,6V$).


## BMS

Un BMS est à cabler sur chaque cellules en série.

![BMS 4S](/assets/posts/learning-batt-lithium/bms-4s.jpg){: width="400" }
_Exemple de branchement d'un BMS 4s_

Les objectifs sont les suivants :
- Faire en sorte que toutes les cellules en série restent à la même tension
- Il premet de rééquilibrage en vidant le trop plein d'énergie de certaines cellules atteignes le seuil de tensions haut avant les autres
- Empècher la sous-charge des cellules
- Empècher la surcharge des cellules
- Détecter les court-circtuis

Pour choisir son BMS :
- Il faut connaitre le nombre de cellules en série
- Il faut connaitre le courant de charge (charge current) et de décharge (continue discharge current)
- Il faut connaitre la tension de charge (charge voltage)
- Il faut connaitre les plages de tension des cellules (over charge / over discharge)

Dans la datasheet du BMS on peut également retrouver la tension d'équilibrage (balance), mais également le contrôle de la température.

Et c'est là où c'est la merde avec les BMS tout préparé du commerce... Deux solutions : Faire notre propre BMS (Mais j'ai pas encore le niveau...); Soit utiliser un BMS paramétrable (smart BMS).


## Chargeur de batterie

TODO


## Cas d'un circuit de protection d'une batterie d'une seule cellule

TODO

Quelques pistes à creuser :
- BMS 1 cell : TC4056 / AP9101C / LTC4056
- Equivalent : MCP73833 https://www.farnell.com/datasheets/1669398.pdf


## Montage d'une batterie

Avant de monter la batterie, il faut charger chaque cellule pour qu'elles atteignent toutes la même tension.

Il existe des spacers de batterie (support d'entretoise de batterie). Exemple : [nkon.nl](https://www.nkon.nl/fr/accessories/packaccessoires/spacer.html)

Il existe plusieurs techniques d'asssemblage possible :

1. Soudage par point
    Un soudage par point peut se faire en utilisant un "spot welder" (soudure par point). Cela permet de ne pas endommager les cellules car ce type de soudage ne fait pas chauffer les cellules.

2. Barre de cuivre
    La soudure des électrode se fait à l'étain. Pour y arriver, il faut bien poncer les électrode (y aller à coup de papier de verre + dremel)
    
    > Pensez à utiliser une pince non conductrice pour éviter les cours-circuit entre les batteries, le fer à souder et la pince qui tient le fil à souder.
    {: .prompt-warning }

3. Fils fusible
    La soudure des fils fusible se fait comme pour les barres de cuivre.
    L'avantage des fils fusible est d'ajouter une protection contre les court-circuits pour chaque cellule (avant les protections BMS).
    Il n'est pas nécessaire de mettre du fil fusible sur les deux électrodes d'une cellule. Cela fait doublon. On peut se limiter à un fil fusible sur une seule électrode.

Dans tous les cas, il faut bien dimensionner les fils/rubans servant aux jonctions entre les cellules.
Pour ce faire, il faut partir du courant max que la batterie peut fournir et déterminer pour chaque jonctions le courant qui va la traverser.

Exemple avec un 4s3p pour laquelle le courant max est de 10A :

![Courant max par jonctions](/assets/posts/learning-batt-lithium/max-current.png){: width="400" }
_Courrant maximum par jonction_

> Pour le cas des fils fusible, il faut bien les dimensionner pour qu'ils ne lâchent pas en cas de courant en contniue, mais ils doivent bien lâcher en cas de court-circuit.
> En règle générale, on prend une valeur de coupure deux fois supérieure au courant max. Exemple : Si une jonction peut atteindre $3.33A$, alors on prend un fil fusible de $7A$.
{: .prompt-warning }

Dans tous les cas, il faut bien vérifier que les cables/rubans sont correctement dimensionnés pour l'assemblage.

Pour celà, il faut déterminer le courant qui va passer entre chaque jonction des cellules et vérifier que le conducteur est suffisament dimentionné en s'aidant du tableau de conductibilité suivant :

![Conductivité des rubans](/assets/posts/learning-batt-lithium/conductivity-metal-strip.jpg){: width="400" }
_Tableau de la conductivité des rubans en fonction de leur composition_

> Astuce pour connaitre la section en $mm^2$ d'un cable monobrun :
>
> Rappel du calcul de la surface $S$ d'un cercle : $S = R^2 \times \pi$
>
> On récupère le diamètre $D$ du cable avec un pied à coulisse, ainsi $R = \frac{D}{2}$.
>
> Exemple pour un cable de diamètre $D = 1.78mm$ :
>
> $R = \frac{1.7}{2} = 0.89mm$
> $\Rightarrow S = 0.89^2 \times \pi \approx 2.49 mm^2$
{: .prompt-tip }

Une fois les cellules assemblées, il faut ajouter un emballage de la batterie. Pour ce faire, nous pouvons utiliser le plastique therme durcissable. Ou sinon du gros scotch orange...


## Différencier rubans de nikel pur et ruban d'acier nikelé

Le nikel conduit mieux le courant que l'acier.

Acier nikelé = Ruban d'acier recouvert d'une fine couche de nikel.

Techniques :

1. Faire apparaitre de la rouille (l'acier ouille mais pas le nikel)
    Prendre un échantillon du ruban et gratter sa surface.

    Préparer une solution eau + sel et mettre l'échantillon dedans.

    Attendre 1 ou 2 jours pour voir si de la rouille apparait.

2. Meuler le ruban (dremel) et voir si l'on a l'apparition d'étincelles.
    S'il y a des étincelles c'est de l'acier.


## Taille des cellules

Le type de cellule donne directement sa taille.

Exemple :
- Une cellule 18650 mesure 18mm de diametre pour 65mm de haut
- Une cellule 21700 mesure 21mm de diametre pour 70mm de haut


## Remplacement de batterie

En cas de remplacement d'une batterie existante (sans changement de BMS) :

- On ne peut pas changer le format (la taille) des cellules sinon il n'y aura pas la place
- Il faut trouver une cellule avec au moins la même capacité nominale (ou plus si l'on veut une meilleure capacité)
- Pour la charge :
    + Courant de charge (constant current) doit être au moins égale à la cellule d'origine (la nouvelle cellule doit pouvoir supporter le courant de charge du chargeur)
    + Tension de charge (constant voltage) doit être rigoureusement identique (car on ne modifie par le chargeur)
- La tension de coupure doit être identique (car géré par le BMS que l'on ne change pas)
- Le courant de décharge constant et en crète doit être au moins égale à la cellule d'origine


Site pour trouver des batteries :
- https://www.akkuteile.de
- https://www.nkon.nl/fr/

> NE PAS PRENDRE DE BATTERIE SUR ALIEXPRESS
{: .prompt-danger }

> Lors du remplacement des cellules, il peut y avoir une sonde thermique. Il convient de remettre de la pâte thermique si necessaire.
{: .prompt-info }


## Comment bien mesurer la capacité restante d'une batterie

Capacité = quantité d'energie emmagasiné stocké et restitué par une cellule sous certaines conditions.

La capacité d'une batterie va varier en fonction des conditions de charge et de décharge : Plus on va tirer du courant d'une batterie, plus sa capacité va diminuer au cours du temps.

Donc pour bien mesurer la capacité restante d'une batterie, il faut suivre le même mode opératoire que celui indiqué dans la datasheet.

Pour celà, il faut utiliser une charge électronique.
NOTE : Il existe également des testeur de batterie (Ex : EBC-A20) permettant de paramétrer des cycles de charges / décharges le tout piloté par un ordinateur.

> Une batterie perd en capacité lors d'un stockage long terme.
{: .prompt-info }


Etude sur le veillissemnt des batteries : https://core.ac.uk/download/pdf/144283017.pdf
