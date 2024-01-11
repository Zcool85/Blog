---
title:  "Controller de volet roulant Somfy RTS"
type: Domotique
last_modified_at: 2024-01-07 21:52:00 +0100
state: En cours
math: true
---

Mon objectif sur ce projet est de pouvoir controler tous mes volets roulant depuis
un seul et même appareil.

<!--more-->

Dépôt GitHub : [https://github.com/Zcool85/SomfyHome](https://github.com/Zcool85/SomfyHome){:target="_blank"}

> Disclaimer : Les informations fournies ici sont à titre informatif uniquement. Elles ne
> constituent pas des conseils professionnels. Je ne peux pas garantir l'exactitude, l'exhaustivité,
> ou la pertinence des informations fournies.
> Je ne pourrais être tenu pour responsable de l'usage des informations de ce projet. Je ne pourrais
> en aucun cas être tenu responsable de tout domage direct, indirect, consécutif ou autre découlant
> de l'utilisation des informations de ce projet ou de toute information obtenue à partir de celui-ci.
{: .prompt-warning }

## Mon matériel Somfy

Je dispose de 8 volets roulant, chacun ayant leur propre télécommande "Centralis RTS". De plus, je
dispose d'une télécommande "Telis 4 RTS" générale, mais cette dernière est limitée au pilotage
simultanée de 4 volets maximum.

Chaque volet roulant mémorise les télécommandes qui peuvent le piloter. Chaque volet
roulant peut mémoriser jusqu'à 12 télécommandes.

Doc pour [Centralis RTS](https://service.somfy.com/downloads/fr_v5/noticecentralis_rts.pdf).

Doc pour [Telis 4 RTS](https://service.somfy.com/downloads/fr_v5/noticetelis_1_rts.pdf).

Quelques docs contenant pas mal d'astuces :

- [Enregistrer une télécommande Telis 4 RTS depuis des télécommandes individuelles](https://service.somfy.com/downloads/fr_v5/programmationde_la_telecommande_telis_4_rts_depuis_des_telecommandes_individuelles.pdf)
- [Enregistrer une télécommande Telis 4 RTS depuis un point de commande Inis RT](https://service.somfy.com/downloads/fr_v5/programmationde_la_telecommande_telis_4_rts_depuis_un_point_decommande_inis_rt.pdf)
- [Aide au patamétrage](https://service.somfy.com/downloads/fr_v5/aideau_parametrage_telis_4_rts.pdf)
- [Remplacer une Telis 4 sans autre télécommande](https://service.somfy.com/downloads/fr_v5/changementde_telecommande_telis_4_rts_sans_autres_telecommandes.pdf)
- [Copier une Telis 4 RTS sur une nouvelle Telis 4 RTS](https://www.somfy.fr/fr_v5/fr/fr-fr/file.cfm?contentid=301424)

Doc pour [désappairer](https://service.somfy.com/downloads/fr_v5/supprimerune_telecommande_vr.pdf)
une télécommande.

## Choix du hardware

Pour ce projet, j'ai décidé d'utiliser un ESP32 pour pouvoir envisager d'utiliser
mon réseau local pour, par la suite, pouvoir piloter les volets roulant depuis mon
téléphone ou depuis internet.

Pour pouvoir communiquer avec les volets en RTS, j'ai décidé d'utiliser le module
RFM69HCW (module qui semble le plus simple à utiliser pour ce type de projet). De plus,
de nombreux projets utilise ce type de module pour faire fonctionner leur propre
volet roulant Somfy.

Bien entendu, ce projet me permettra de me familiariser avec les ESP32 et l'usage
de leur outil de développement ESP-IDF.

## Branchements

Voici le pinout du module RFM :

![Broches du module RFM69HCW](/assets/projects/SomfyRTS/RFM69HCW_pinout.png){: width="250" }
_Brochage du module RFM68HCW_

l'ESP32 que j'utilise est une des première verison montée sur plaque de développement:

![Broches du module de dev ESP32](/assets/datasheets/esp32/esp_wroom32_breakout_pin.jpg){: width="500" }
_Brochage du module de dev ESP32_

J'ai effectué le branchement suivant :

| Pin RFM69HCW | Pin ESP32
|--------------|----------
| MISO         | 31 (GIOP19)
| MOSI         | 37 (GIOP23)
| SCK          | 30 (GIOP18)
| NSS          | 29 (GIOP5)
| DIO2         | 23 (GIOP15)
| RESET        | GND

> J'ai "bêtement" utilisé les broches du VSPI du module ESP32. Cela me permet d'utiliser
> le code par défaut fourni par Espressif pour gérer le protocol SPI avec le module ESP32.
{: .prompt-info }

> J'ai placé un condensateur de 0.1uF entre les broches 3.3V et GND du module RFM69HCW comme
> préconisé dans la datasheet.
{: .prompt-info }

> J'ai directement utilisé l'alimentation 3.3V du kit de dev ESP32 pour alimenter le RFM69HCW.
{: .prompt-info }

![Plaque a essaie](/assets/projects/SomfyRTS/Beadboard.jpg){: width="500" }
_Plaque a essaie utilisée_

## Analyse du protocole

Avant de suivre "bêtement" d'autres projets sur le net pour piloter des volets
roulant Somfy RTS, je cherche dans un premier temps à analyser et comprendre le
protocole Somfy.

Pour ce faire, je me suis mis à l'implémentration d'un "sniffer" de trame Somfy.
Je ne suis pas partie de la page blanche et je me suis inspiré par les exmplications
du protocole faite par [PushStack](https://pushstack.wordpress.com/somfy-rts-protocol/).

J'ai vraiement __bien__ galéré sur le bon paramétrage du module RFM69HCW en mode
réception OOK... Et la datasheet n'est pas super explicite sur la manière d'utiliser
ce mode.

### Paramétrage du module RFM69HCW

Voici donc le paramétrage fonctionnel chez moi pour intercepter correctement
les signaux de mes télécommandes :

1. SPI
   
   Le module RFM69HCW se configure via SPI. Rien de bien méchant car c'est plutôt
   standard :
    - SPI mode 0 (CPOL=0; CPHA=0)
    - Clock speed 5 MHz
    - 7 bits d'adresse
    - MSB first

    > Une fois le SPI configuré, j'ai pris le temps de vérifier que la communication
    > avec le RFM69HCW était fonctionnelle en allant lire le registre RegVersion (0x10)
    > permettant de récupérer la version du module. On peu alors vérifier que l'on
    > obtient bien la valeur 0x24 comme spécifier dans la datasheet.
    {: .prompt-tip }

2. Modulation
   
   Il faut configurer le module pour fonctionner en mode continue sans synchronisation
   en modulation OOK. L'usage de la synchronisation ne fonctionne pas. Mon interprétation
   est que le signal que l'on cherche à analyser n'a pas de timing "constant". En effet, avant
   la partie data qui nous intéresse, il y a des trames avec un timing bien précis à respecter
   avant l'envoie des data. Ce timing n'étant pas le même que la data, la synchronisation ne
   peut pas se faire.

   Données du registre RegDataModul (0x02) :

    | Bits | Valeur | Signification
    |------|--------|--------------
    | 7    | -      | Non utilisé
    | 6-5  | 11     | Mode continue sans bit de synchronisation
    | 4-3  | 01     | Typoe de modulation OOK
    | 2    | -      | Non utilisé
    | 1-0  | 00     | Pas de shaping

    > Je n'ai pas constaté de différence entre les différents shaping possibles.
    {: .prompt-info }

3. Bitrate
   
   La datasheet n'est pas super claire à mon goût sur l'utilité de ce paramètre. Toutefois,
   d'après mes tests empiriques, si le bitrate est trop élevé, les impulsions ne sont pas "propre"
   et j'ai de nombreuses fausses lectures :
   
    ![Bitrate trop élevé](/assets/projects/SomfyRTS/bitrate_trop_eleve.png){: width="500" }
    _Bitrate trop élevé : Détections de fronts montant/descendant non désirés_
   
   A l'inverse, avec un bitrate trop bas, je perd la plupart des impulsions :

    ![Bitrate trop bas](/assets/projects/SomfyRTS/bitrate_trop_bas.png){: width="500" }
    _Bitrate trop bas : Certains fronts montant/descendant sont perdus_
   
   J'en ai donc déduit que le bitrate est en quelque sorte la fréquence des lectures internes du module.
   
   Je suis reparti de l'analyse de [PushStack](https://pushstack.wordpress.com/somfy-rts-protocol/) et j'ai
   décidé de calculer un bitrate correspondant au nombre de bit maximum qui peuvent être transmis par seconde
   avec l'encodage manchester : Un bit est transmis en $1204\,\mu s$; Il est donc possible de transmettre
   $\frac{1}{1204 \times 10^{-6}} \approx 830\,bits/s$. En utilisant ce bitrate, mes lectures sont correctes,
   toutefois parmis toutes les trames que j'ai visualisées, j'ai constaté qu'un bit peut être transmis
   entre $1204\,\mu s$ et $1392\,\mu s$... J'ai coupé la poire en deux, et j'ai donc décidé de me baser
   sur une transmission de $1300\,\mu s$ par bit, soit $\frac{1}{1300 \times 10^{-6}} \approx 770\,bits/s$.

    > Les tolérances doivent être assez grandes car je n'ai pas constatés de différence entre un bitrate
    > à 830 vs un bitrate à 770... Mais la vérité doit se trouver quelque part par là :blush:
    {: .prompt-info }

   La datasheet indique qu'il faut appliquer la formule suivante pour déterminer la valeur des registres
   RegBitrateMsb (0x03) et RegBitrateLsb (0x04) : $$\frac{F_{XOSC}}{BitRate}$$.

   $$F_{XOSC}$$ vaut $$32\,MHz$$, donc la valeure des registre doit être fixée à $$\frac{32 \times 10^{6}}{770} \approx 41558$$.

   En binaire sur 16 bits, $$41558 = 1010\;0010\;0101\;0110$$ (ou 0xA256 en hexa).

   On obtient donc pour le registre RegBitrateMsb (0x03) :

    | Bits | Valeur              | Signification
    |------|---------------------|--------------
    | 7-0  | 1010 0010 (ou 0xA2) | MSB de la valeur du bitrate

   Et pour le registre RegBitrateLsb (0x04) :

    | Bits | Valeur              | Signification
    |------|---------------------|--------------
    | 7-0  | 0101 0110 (ou 0x56) | LSB de la valeur du bitrate

4. Bandwith de réception

   Pour ce paramètre, j'ai testé de manière empirique les valeurs proposées
   dans la datasheet (Table 14). J'ai eu de bons résultats avec la valeur 10,4 kHz (Modulation OOK).

    > En revanche, j'ai eu de mauvais résultats en utilisant une largeur de bande plus basse comme
    > par exemple avec la valeur par défaut de 5.2 kHz.
    {: .prompt-info }

   Données du registre RegRxBw (0x19) :

    | Bits | Valeur | Signification
    |------|--------|--------------
    | 7-5  | 010    | DCC (Cut-off frequency)
    | 4-3  | 10     | Channel filter bandwidth control (10 -> RxBwMant = 20)
    | 2-0  | 100    | 10.4 kHz

5. OOK Peak & Fix threshold

   De ma compréhension, ce paramètre permet de définir le seuil de détection des modulations OOK.

   Mes essais en mode "fixed" n'ont jamais été concluant :
   - Soit je me retrouve avec pleins de parasides si le threshold est bas (moins de 10 dB)
   - Soit certains fronts montant ou descendant sont perdus si le threshold est trop haut (au dela de 30 dB)
   - A 20 dB j'ai périodiquement quelques parasites ou quelques fausses lectures

   De même avec le mode "average" où j'ai tout le temps de la friture sur la ligne.

   Le seul type de seuil que j'arrive à faire marcher c'est le type "peak" avec toutes les valeurs
   par défaut.

   Données du registre RegOokPeak (0x1B) :

    | Bits | Valeur | Signification
    |------|--------|--------------
    | 7-6  | 01     | threshold type "peak"
    | 5-3  | 000    | Taille du décrément à 0,5 dB
    | 2-0  | 000    | Périod du décrément à "once per chip"

   Pour une raison que j'ignore, l'alimentation du registre "RegOokFix" change le comportement de
   la réception même si l'on est en threshold type "peak".

   Dans mon cas, les meilleurs résultats ont été obtenu en fixant le registre "RegOokFix" (0x1D) à
   une valeur de 30 dB.

   Données du registre RegOokFix (0x1D) :

    | Bits | Valeur          | Signification
    |------|-----------------|--------------
    | 7-0  | 00011110 (0x1E) | Seuil du mode "fixed" en dB (0x1E = 30 dB)

6. Fréquence

   La fréquence à utiliser pour communiquer avec les volets Somfy est de $$433,42\,MHz$$.

   La datasheet indique la formule à suivre pour alimenter lees registres RegFrfMsb (0x07),
   RegFrfMid (0x08) et RegFrfLsb (0x09). Il faut au préalable connaitre la fréquence du synthétiseur.
   Une nouvelle fois la datasheet nous indique comment calculer sa valeur : $$F_{step} = \frac{F_{XOSC}}{2^{19}}$$.
   
   $$F_{XOSC}$$ vaut $$32\,MHz$$, donc la valeure de $$F_{step} = \frac{32 \times 10^{6}}{2^{19}} \approx 61 Hz $$.

   A partir de là, il ne reste qu'à déterminer la valeur du registre à positionner, soit, toujours d'après
   la datasheet : $$F_{rf} = \frac{433,42 \times 10^{6}}{F_{step}} \approx 7105245$$.

   En binaire sur 24 bits, $$7105245 = 0110\;1100\;0110\;1010\;1101\;1101$$ (ou 0x6C6ADD en hexa).

   Données du registre RegFrfMsb (0x07) :

    | Bits | Valeur          | Signification
    |------|-----------------|--------------
    | 7-0  | 01101100 (0x6C) | MSB de la fréquence

   Données du registre RegFrfMid (0x08) :

    | Bits | Valeur          | Signification
    |------|-----------------|--------------
    | 7-0  | 01101010 (0x6A) | Octet central de la fréquence

   Données du registre RegFrfLsb (0x09) :

    | Bits | Valeur          | Signification
    |------|-----------------|--------------
    | 7-0  | 11011101 (0xDD) | LSB de la fréquence

7. Calibration

   Pour la gloire de la horde, j'effectue aussi une calibration de l'oscillateur (mais je doute
   fort que celà est un impacte fort sur la lecture).

   Je positionne donc le bit n°7 du registre "RegOsc1" (0x0A) permettant de déclancher
   la calibration de l'oscillateur (ATTENTION : Cette calibration ne peut se faire qu'en mode
   standby).

   Puis, j'attends patiemment que le bit n°6 de ce même registre soit positionné à 1. Cela indique
   que la calibration est terminée.

8. Activation de la réception (mode RX)

   Enfin, j'active le module en mode réception. Pour celà, il suffit de positionner les bits n°4 à 2
   avec la valeur 0b100.

   Puis, j'attends que le bit n°7 du registre "RegIrqFlags1" (0x27) soit positionné à 1. Cela indique
   que le module a bien fini de basculer dans le mode de fonctionnement demandé.

### Paramétrage de mon analyseur logique

Malgré la mise en place de tout ce paramétrage, j'ai été obligé d'utiliser mon petit
analyseur logique pour pouvoir visualiser les trames du module RFM69HCW...

Voici la configuration qui marche chez moi avec mon analyseur logique :

- Mode : Manchester
- Bit rate : 770 bits/s
- Edge polarity : negative edge is binary zero
- Bits per Frame : 8
- Significant bit : MSB first
- Preamble bits to ignore : 1
- Tolérance : 25% de la période

![Configuration de mon analyseur logique](/assets/projects/SomfyRTS/Config_Logic2.png)
_Configuration de mon analyseur logique_
   
Et là, bingo, tout est nikel :

![Exemple de trame lue](/assets/projects/SomfyRTS/Exemple_trame.png)
_Exemple de trame lue_

> Cette trame a été lue depuis une télécommande "Telis 4 RTS".
{: .prompt-info }

### Interprétation d'une trame

Maintenant que l'on arrive à récupérer une trame bien propre, je me suis atelé au décodage de celle-ci.

Pour le coup, les travaux de [PushStack](https://pushstack.wordpress.com/somfy-rts-protocol/) sont très bien
et je n'ai que peu de chose à ajouter si ce n'est que chez moi je constate des timing toujours un peu plus long
que les siens.

Quelques exemples :

- L'impulsion de "wakeup" est plutôt de l'ordre de 10,4ms chez moi
- Les impulsions de "hw.sync." sont plutot de l'ordre de 2,6ms
- Les symboles ont une impusion d'environ 1300/1350us

> Il est tout a fait plausible que ces écarts soient liés à mon équipement (chinois à pas cher) qui est
> très certainement moins précis, ou alors à la télécommande que j'ai utilisée.
{: .prompt-info }

### Cas des télécommandes "Centralis RTS"

Par curiosité, j'ai été analyser une trame envoyée par mes télécommandes positionnée juste à côté de
mes fenêtres (des "Centralis RTS")... Et là, stupeur ! Les trames sont bien différentes :

![Exemple de trame Centralis](/assets/projects/SomfyRTS/trame_centralis_rts.png)
_Exemple de trame Centralis_

On retrouve bien toutes les phases, mais cette fois-ci, on se retrouve avec des impulsions au lieu de niveaux
logiques maintenus... Comme si chaque front montant et descendant du protocol manchester était chacun
représenté par une impulsion.

L'algorithme pour que mes lectures soient compatibles entre les deux téélcommande va être plus complexe
que prévu...

## Liens externes et documentations

Documentations générales des [Volets roulant Somfy](https://www.somfy.fr/assistance/notices/volet-roulant/radio-rts).

Explication du [protocole](https://pushstack.wordpress.com/somfy-rts-protocol/)
Somfy RTS.

Exemple de [bibliothèque C++](http://www.airspayce.com/mikem/arduino/RadioHead/index.html)
pour coder une librairy dédiée au RFM69HCW.

Usage de ce [projet](https://github.com/etimou/SomfyRTS/tree/master)
pour extraire le bon réglage du module.
