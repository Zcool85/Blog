---
title:  "Compte tour roue de hamster"
type: Fun
last_modified_at: 2012-03-17 21:55:00 +0100
state: Terminé
---

Tout est dans le titre !

<!--more-->

Comme toutes bonnes idées, les meilleures sont souvent les plus connes ! :yum:

Depuis mi-décembre je planche sur un petit projet marrant à réaliser : Un compte-tour
de hamster ! Cette idée m'est venue grâce à Sirerobert qui n'arrive pas à nous dire
combien de tour de roue fait son hamster la nuit (oui, ça fait du bruit un hamster
qui court dans sa roue... Surtout la nuit !). Bref, pour répondre à cette question que
tout le monde se pose (me dite pas que vous ne vous l'êtes jamais posée, je ne vous
croirais pas !) : Nous avons décidé de construire un hamster's roll !

## Schéma et principe de fonctionnement

Voici donc les schémas de cette réalisation. On commence par l'alimentation qui me
permet de délivrer une tension de 9 V en continue (J'avais aussi la possibilité
d'utiliser une pile plate de 9V, mais l'idée de la changer tous les 4 matins ne me
plait pas... Et c'est pas très écolo...) :

![Alimentation](/assets/projects/HamsterRoll/Hamster-roll-Alimentation.jpg){: width="400" }
_Alimentation_

La seconde partie concerne le capteur permettant d'envoyer une impulsion au compteur
à chaque tour de roue. Ici, l'idée est d'utiliser un capteur infrarouge avec une
feuille de papier photo (qui réfléchit la lumière sur le capteur) :

![Capteur](/assets/projects/HamsterRoll/Hamster-roll-Trigger.jpg){: width="400" }
_Capteur_

La troisième partie concerne le compteur en lui même. Ce compteur est basé sur des
CD4033. 4 compteurs sont utilisés permettant de compter jusqu'à 9999 tours :

![Compteurs](/assets/projects/HamsterRoll/Hamster-roll-Counters.jpg){: width="400" }
_Compteurs_

La dernières partie concerne les afficheurs qui sont ni plus ni moins que des
afficheurs 7 segments. La seule particularité est l'utilisation d'un NE555 pour faire
"clignoter" les afficheurs afin de réduire la consommation électrique (divisée par
deux). Je vous rassure, le clignotement est tellement rapide que l'œil humain ne le
voit pas. Les afficheurs semblent donc toujours allumés :

![Afficheurs](/assets/projects/HamsterRoll/Hamster-roll-Display.jpg){: width="400" }
_Afficheurs_

Et voilà ! Ce circuit a été testé sur plaque à essai et fonctionne très bien ! Il ne
reste plus qu'un exploit à réaliser : Tout faire tenir dans une boite de 15 cm x 8 cm
x 5 cm...

Un grand merci à Shoeur pour ces prouesses physiques ! Tous les hamster s'en
souviendront !! :grin:

![Shoeur en pleine action ! :grin:](/assets/projects/HamsterRoll/IMG_0434.jpg){: width="400" }
_Shoeur en pleine action ! :grin:_


## Conception PCB

Après un moment passé sur Radiospares pour choisir les composants désirés (je
posterais la netlist bientôt), j'ai donc naturellement attaqué la conception du PCB.

Après deux essais foireux, j'ai enfin réussi à avoir une version qui me convient à
peut prêt... Pas évidant de tout caser en si peu d'espace en minimisant les risques à
la gravure :neutral_face:

Attention, il y a bien deux PCB : Un pour le transformateur et des composants de
comptage, un autre pour les afficheurs 7 segments.

Voici donc l'emplacement des composants sur les plaques :

![Emplacement des composants](/assets/projects/HamsterRoll/Hamsters-roll-Position-composants.jpg){: width="400" }
_Emplacement des composants_

Et plus important encore, voici les typons des deux plaques en 300 dpi :

![Typons](/assets/projects/HamsterRoll/Hamsters-roll-Typon.bmp){: width="400" }
_Typons_

Sur ce typon j'aurais un challenge de taille ! C'est la première fois que je vais
graver des pistes de 0.8 mm espacées de 0.5 mm ! J'espère que tout se passera au
mieux...

Allez, je vais mettre mes gants et jouer au terroriste avec des produits
chimiques... :grin:


## Réalisation du PCB

Ça y est, les plaques sont gravées ! Plusieurs nouveautés par rapport à ce mes
habitudes :

Plusieurs pistes de 0.8 mm espacées de 0.5 mm existent... Je suis d'habitude au dessus
du millimètre...
Je test une nouvelle marque de plaques pré-sensibilisés (donc les temps
d'insolations / gravure sont forcément différent).
Je test également l'application d'un vernis pour que le circuit s'oxyde moins
rapidement... J'utilise une bombe de vernis vert KF 1451... Il parait qu'on peut
encore souder dessus après application...
Et bien en fait, malgré ces nouveautés, je suis content du résultat obtenu. Voyez
plutôt :

![Gravure circuit d'allimentation + comptage](/assets/projects/HamsterRoll/IMG_0435.jpg){: width="400" }
_Gravure circuit d'allimentation + comptage_

![Gravure des afficheurs](/assets/projects/HamsterRoll/IMG_0436.jpg){: width="400" }
_Gravure des afficheurs_

![Alimentation et compteur percé](/assets/projects/HamsterRoll/IMG_0437.jpg){: width="400" }
_Alimentation et compteur percé_

Allez, juste pour le fun, vérifions que je ne me suis pas trop planté sur les mesures
des afficheurs 7 segments :

![Classe non ? :grin:](/assets/projects/HamsterRoll/IMG_0438.jpg){: width="400" }
_Classe non ? :grin:_

Bon, alors là je passe pas mal de détails... Car en fait, j'ai loupé les dimension du
transfo... Donc j'ai un peu plus d'un millimètre à rattraper... J'ai cherché
longuement la visserie pour fixer le fusible sur le circuit... Et je cherche toujours
la visserie pour le bouton de remise à zéro !!

Bref, j'ai avancé le soudage des composants sur le circuit. Autant dire que j'en est
chier grave sa mère (si si, là j'ai le droit de le dire...) !! Alors, pour les
composants de bases rien à dire c'est facile... Et le vernis ne gène pas. Donc pour
les réalisations qui doivent tenir quelques années, je conseil l'utilisation du KF
1451 :slightly_smiling_face: Par contre les liaisons des afficheurs, c'est trop
galère ! La prochaine fois, je me trouve des embases pour CI. C'est trop chiant de
faire passer ces foutus câbles dans des trous de 0,5 mm. Rien que ça j'ai du y passer
plus d'une heure !! Et les pistes de 0,8 mm ça tient pas longtemps la chaleur... J'en
ai bousillé une d'ailleurs :sob: Mais au final, ça marche pas si mal !

J'ai pas pu résister à l'envie de tester le montage :

![Dernier test avant mise sous boitier...](/assets/projects/HamsterRoll/IMG_0441.jpg){: width="400" }
_Dernier test avant mise sous boitier..._

Et voilà ! Moi je trouve que ça le fait quand même :grin: On est jamais trop fier de
ces créations :smirk:.

Bon et bien il ne reste plus que la boite à finir... Percer les trous pour le câble
d'alim, pour le bouton de remise à zéro, pour le réglage de l'intensité lumineuse,
pour le capteur et pour les afficheurs... Bon ok, il y a encore un peu de boulot pour
la finition. Mais ça vient bon !! :grin:


## Le montage final

Et voilà, la boite est faite ! Je passe les détails sur la réalisation car il ne
s'agit ici que de percer des trous et coller quelques éléments.

Voici quelques photos :

![Vue de face](/assets/projects/HamsterRoll/IMG_0443.jpg){: width="400" }
_Vue de face_

![Le chat qui se demande ce que c'est...](/assets/projects/HamsterRoll/IMG_0444.jpg){: width="400" }
_Le chat qui se demande ce que c'est..._

![Vue de côté](/assets/projects/HamsterRoll/IMG_0447.jpg){: width="400" }
_Vue de côté_

Il ne reste plus qu'à aller dans le centre de la France pour installer le boitier sur
la roue de Shoeur :grin:.

Sire, on peut passer quand ???

