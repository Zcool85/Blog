---
layout: post
author: zcool
title:  "Premier circuit imprimé 'maison'"
categories: [Electronique, Circuits imprimés]
---

Avant toute chose, je me permettrais d'employer l'abréviation PCB (Printed Circuit Board en Anglais) pour désigner un circuit imprimé (ou CI en Français). J'utilise des plaque PCB présensibilisées pour faire mes circuits imprimés.

En premier lieu, je vais expliquer les étapes de création d'un PCB :

- Création du typon
- Insolation du circuit imprimé
- Révélation du circuit
- Gravure du circuit
- Perçage des trous
- Étamage
- Vernissage

Puis, je donnerais quelques trucs et astuces sur l'utilisation des produits chimiques.

## Création du PCB

### Création du typon

Le typon est en fait le négatif de la partie cuivrée du circuit imprimé. Il s'agit donc de créer les liaisons des pistes cuivrées sur le circuit imprimé. Pour ce faire, j'utilise encore et toujours le logiciel DipTrace. Ce dernier permet de positionner les composants sur la plaque et de tracer les liaisons électriques entre les composants. Je ne détaillerais pas ici l'utilisation du logiciel car tout le moment n'utilise pas forcément le même et son utilisation est détaillée dans la documentation du logiciel.

Je vais simplement lister des astuces permettant de créer des typons propres et éviter les problèmes d'interférences.

En tout premier lieu, il est conseillé de garder un maximum de cuivre sur le PCB pour:
- Éviter d'avoir à faire un montage en étoile. Les montages en étoiles permettent d'éviter les interférences liées aux composants reliés à la masse. Le principe est de relier ces composants via des circuits formant une étoile. Cette pratique est un peu pénible et impose une disposition particulière des composants sur le circuit. Nous verrons qu'il est possible de ne pas utiliser cette technique tout en évitant les interférences liées à la masse.
- Économiser le perchlorure de fer. En effet, moins il y a de cuivre à enlever, plus le perchlorure tient longtemps.
- Graver plus vite les circuits. Encore une fois, moins il y a de cuivre à enlever, plus la gravure est rapide.

Le logiciel DipTrace dispose d'une fonction nommée "Cutter pour" qui permet de faire ce que l'on appel des plans de masses. Le principe est de combler les zone sans cuivre avec... du cuivre ! Il y a la possibilité de relier ces zones à la masse pour former un plan de masse.

Dans certaines situations, il est possible que des "îles" se forment. Ces îles sont des zones de cuivres non reliées. Il faut toujours penser à supprimer les zones de cuivre non reliées (pas de cuivre en l'air sous peine d'avoir des interférences). En gros, dans cette situation, je rapproche les pistes autour de l'île pour la faire disparaitre.

Si le logiciel n'offre pas de fonction "Copper pour", il convient de créer une étoile de masse. Le but étant de créer de nouvelles branches de masse pour chaque sous-ensemble du circuit. L'étoile ainsi formée doit se trouver au plus proche de la masse globale du circuit. Si cette étoile n'est pas faite, un phénomène dit "de ronflette" apparait (perturbations sur la ligne d'alimentation). Ce phénomène très connu dans la HiFi.

Voici le paramétrage que j'utilise avec DipTrace pour le tracé des pistes entre :

1. Taille des pastilles pour les soudures
    - Si rectangles (pour les pastilles positive) : 2,5 mm * 2,5 mm
    - Si rondes (pour les résistances ou les masses) : 2,5 mm * 2,5 mm
    - Si ovales (pour les supports DIP) : 1,6 mm * 2,5 mm
    - Pour les boitiers de type TO-220 : ovales 1,8 mm * 2,5 mm
2. Taille des liaisons : 1 mm (il est possible de descendre à 0,5 mm si pas de place, mais la piste risque de disparaître lors de la gravure)
3. Taille des trou : 0,8 mm de diamètre (1 mm pour les boitiers TO-220 et pour les gros condos). Dans tous les cas, il est très conseillé de consulter les documentations des composants pour connaitre le diamètre de leurs pattes...
4. Espace minimum entre les liaisons : 0,6 mm (équivalent au paramètre Clearance du Copper Pour).

Paramétrage de la fonction Copper pour :
- Line width : 0,635 mm
- Line Spacing : 0,635 mm
- Clearance : 0,6 mm

> NOTE : Pour les piste en 220V, j'utilise des pastilles de 5 * 5 mm (si ronde) ou 3,5 * 5 mm (si ovale).
> La taille des liaisons fait 3 mm. De plus, j'utilise un espace de 6 mm entre les liaisons (paramètre Clearance).
> Cela me permet de faire cohabiter sur le même PCB des liaisons 220V avec des liaisons "classiques".
{: .prompt-tip }

L'impression du typon se fait sur papier transparent. Personnellement, j'utilise une imprimante jet d'encre. Attention toutefois, dans cette situation, il ne faut configurer l'impression comme pour du papier ordinaire et non des transparent. En effet, si la configuration est en mode papier transparent, les noirs seront bleutés et plus transparent.

Une petite astuce pour exporter le PCB en image : Toujours utiliser le format BMP à 600 dpi pour l'export. Ensuite, je conseil l'utilisation de Gimp pour convertir l'image en Noir et Blanc et en couleur indexée. Ainsi, la définition de l'image est suffisante pour l'impression d'un typon correcte, et le fichier ne prend presque pas de place sur le disque ! Par exemple, pour un circuit de 6 cm * 12 cm, le fichier ne pèse que 350 Ko.


### Insolation du circuit imprimé

Pour l'insolation du circuit imprimé, j'utilise une valise insoleuse (dont le montage est expliqué dans ce [post]({% post_url 2011-12-07-voulez-vous-bronzer %})).

Le principe de cette étape est de bombarder d'UV la résine collée au cuivre du circuit imprimé à l'endroit où le cuivre devra disparaitre. La résine restante continuera à protéger le cuivre lors de la gravure.

Petite astuce : J'imprime deux fois le typon et je superpose les deux couches. Cela permet de rendre le typon encore plus noir. Le principe est de ne pas laisser passer les UV là où le cuivre doit rester. Si besoin, j'utilise un marqueur à encre noir pour combler les imperfections éventuelles. Attention, j'utilise le marqueur sur l'envers du typon pour ne pas décoller l'encre imprimée.

Pour cette étape :

- J'ouvre la valise
- Je pose le typon sur la plaque de verre
- J'enlève l'opercule du circuit imprimé qui empêche les UV d'attaquer la résine pendant le stockage de la plaque (film protecteur anti-UV)
- Je dépose le circuit imprimé côté cuivre sur le typon
- Je ferme la valise et la branche
- J'attends 2 minutes
- Je débranche la valise
- Une fois l'insolation terminée, je plonge la plaque dans le révélateur.


### Révélation du circuit

> ATTENTION : GANTS OBLIGATOIRES sous peine de dépigmentation de la peau.
{: .prompt-danger }

Le but de cette étape est double :

- Fixer la résine non insolée sur le cuivre
- Supprimer la résine insolée pour laisser à nue le cuivre à supprimer lors de la gravure

La révélation se fait dans une bassine en PVC ou verre dans lequel le révélateur a été déposé. La révélation se voit facilement (celà prend entre 30 secondes et une minute environ). Il suffit de faire quelques vagues avec la cuve pour que la révélation se fasse (en gros c'est comme pour la révélation des photos).


### Gravure du circuit

> ATTENTION : GANTS, AÉRATION et TENUE APPROPRIÉE OBLIGATOIRES sous peine de respirer des vapeurs nocives (gaz moutarde) et de se tâcher.
{: .prompt-danger }

Le but de cette étape est de supprimer les surfaces de cuivre inutiles pour ne laisser que les pistes du circuit imprimé.

Personnellement, j'utilise la méthode de gravure avec du perchlorure de fer. D'autres méthodes existent, mais elles sont encore plus dangereuse, et je ne suis pas encore convaincu d'un meilleur résultat.

IMPORTANT : Il convient de manipuler ce produit chimique en connaissance des risques liés à son utilisation. Donc documentez-vous avant de faire quoi que ce soit. Si vous ne vous sentez pas apte à utiliser ce produit, ne vous lancez pas ! De nombreuses boutiques ou site internet propose la gravure de vos PCB pour des prix plus que convenables...

La gravure se passe en plongeant le PCB révélé précédemment dans du perchlorure de fer légèrement chauffé (32°C maximum).

> A noter : On peut faire chauffer le perchlo vers 28-30°C et couper le chauffage. Le liquide reste tiède assez longtemps. De toute façon, la montée de température ne fait qu'accélérer la gravure... Donc si vous avez le temps, inutile de le chauffer.
{: .prompt-tip }

Il faut dans les 3-4 minutes pour que le circuit soit OK si le perchlo est aux alentours de 30°C. Il ne faut pas hésiter à agiter le circuit dans le bain de perchlo. Il est même conseillé de passer le circuit sous l'eau de temps en temps et de le replonger dans le perchlo.

Puis, après gravure, il faut passer sous l'eau abondante le circuit pour enlever toute trace de perchlo. Le circuit peut alors être séché avec un chiffon ou un mouchoir.

Enfin, il faut enlever la résine restante sur les pistes de cuivre en utilisant le l'acétone. Un circuit correctement gravé doit avoir un ton rosé :

![Circuit OK](/assets/posts/printed-circuits/PCB-OK.jpg)
_Circuit OK_

Si c'est foireux, ça ressemble à ça :

![Circuit foireux](/assets/posts/printed-circuits/PCB-failed.jpg)
_Circuit foireux_


### Perçage des trous

Cette étape consiste à simplement percer les trous nécessaire à implantation des composants sur le circuit imprimé.

Pour percer les trous, j'utilise une Dremelle sur colonne avec un foret au carbure de 0,8 mm. Je déconseille vraiment les forêts de la marque Dremelle. En effet, même si ces forêts coûtent beaucoup moins cher que des forêts au carbure, ils ne tiennent pas ! A peine une centaine de trous et le foret est bon à changer ! L'investissement d'un forêt au carbure est très vite rentable.

> ATTENTION : Les forêts au carbure sont très fragiles (ils cassent au moindre choc). A ne pas laisser tomber donc...
{: .prompt-warning }

> NOTE : Pensez à bien éclairer le plan de travail !
{: .prompt-tip }


### Étamage

Cette étape consiste à ajouter une fine couche d'étain sur les pistes de cuivre afin d'augmenter la durée de vie du circuit imprimé (ah l'oxydation...).

Personnellement, je n'ai encore jamais pratiqué...

Il existe deux méthodes :
- L'étamage à froid qui consiste à plonger le PCB gravé dans un bain d'étain
- L'étamage à chaud qui consiste à déposer de l'étain sur le cuivre avec son fer à souder


### Vernissage

Cette étape consiste une nouvelle fois à allonger la durée de vie du PCB en déposant du vernis sur la face gravée du circuit.

Personnellement, je n'ai pas non plus pratiqué cette étape...

## Trucs et astuces

### Faire son révélateur soit-même

L'idée ici est de faire son propre révélateur moins cher que les sachets disponibles en magasin d'électronique.

La fabrication du révélateur peut se faire à partir de lessive de soude. Ce produit est disponible dans les magasins de bricolage au alentours d'1 € le litre. Il faut utiliser de la soude à 36° ou 30,5 % (la concentration est noté différemment en fonction des fabricants).

Il suffit de verser 4 à 8 cuillères à soupe (un volume de 40-80 cm3) de lessive de soude dans un récipient en verre ou dans un ancien flacon de lessive de soude, puis de compléter avec de l'eau du robinet pour obtenir 1 litre de révélateur prêt à l'emploi. Normalement, avec une bouteille d'un litre de lessive de soude, il est possible de faire plus de 25 litres de révélateur... On a le temps de voir venir !

### Jeter son perchlorure de fer usagé

Le perchlorure de fer saturé se reconnait à sa couleur verte. Il est alors temps de s'en débarrasser !

Pour rappel, il ne faut pas jeter son perchlorure de fer (très corrosif !) dans la nature sans l'avoir au préalable neutralisé. Si vous ne disposer pas d'une déchetterie qui reprend les produits chimiques, il est possible d'utiliser les kit de neutralisation disponibles dans le commerce, mais ils sont assez onéreux... Une autre solution consiste à utiliser de la soude caustique.

Pour se faire, munissez-vous d'un récipient en PVC ou en verre, ainsi que de papier PH. versez le perchlorure à neutraliser dans le récipient, puis ajoutez progressivement la soude caustique et mélanger le tout. Normalement, un dépôt va se former au fond du récipient, c'est de l'hydroxyde de fer, de l'hydroxyde de cuivre insoluble et du sel. Il faut régulièrement vérifier l'acidité de cette solution à l'aide du papier PH. Le but est d'arriver à un PH à 7 (la solution sans soude doit être aux alentours d'un PH 2 ou 3). Une fois le PH à 7, il suffit de laisser s'évaporer la partie liquide de la solution et de jeter le dépôt dans la poubelle.

