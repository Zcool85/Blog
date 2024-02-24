---
layout: post
author: zcool
title:  "Premiers pas avec les microcontrôleurs AVR"
categories: [Electronique, Microcontroller]
---

Pour ce retour après plus d’un an, je me suis décidé à faire un article sur mes premiers pas
avec les microcontrôleurs AVR.

J’ai débuté avec l’ATtiny48PU. Ce dernier est en support PDIP 28 broches (je ne suis pas fan
des composants CMS car je n’ai pas le matos pour les souder… Et encore moins pour graver les
PCB avec des pistes aussi fines !). Bref, ce microcontrôleur est a mon sens très bien pour
débuter pour plusieurs raisons :

- Pas trop onéreux
- Support du protocole SPI
- Support du protocole TWI (ou I2C)
- Possède plusieurs ADC (Analogique Digital Converter)
- 4 Ko de mémoire flash
- 64 bytes d’EEPROM
- 256 bytes de SRAM
- Programmation ISP (In System Programmable)
- Etc...

Je ne vais pas faire toute la liste des trucs sympas, Atmel l’a déjà fait pour moi :
[ATtiny48/88 Datasheet](https://ww1.microchip.com/downloads/en/DeviceDoc/doc8008.pdf){:target="_blank"}.

Après avoir fait quelques essais de programmation en assembleur (faire clignoter une LED),
j’ai très vite basculé dans un univers plus connu pour moi : Le langage C. En effet,
l’assembleur ne m’est pas familier et je n’ai pas le désire aujourd’hui d’apprendre ce langage.

Un point très sympa avec les microcontrôleurs est que les outils de développement sont gratuits !
Il y en a à foison sur Internet... Personnellement, je ne me suis pas embêté, j’ai pris
l'outil de développement proposé directement par Atmel :
[Microchip Studio](https://www.microchip.com/en-us/tools-resources/develop/microchip-studio){:target="_blank"}. Il s’agit d’Atmel Studio 6. Cet outil permet
d’écrire du code en assembleur, en C ou en C++ pour tous les microcontrôleurs AVR. Pour
ceux qui ont l’habitude d’utiliser Visual Studio, je recommande vraiment Atmel Studio 6
qui est en fait un Visual Studio décoré version Atmel.

Enfin, pour programmer un microcontrôleur, il faut un programmateur… Là encore, il existe
une foison d’outils sur le net... Et là encore, je n’ai pas cherché midi à quatorze heure,
j’utilise simplement le programmateur ISP d’Atmel : [AVR ISP MkII](https://www.microchip.com/en-us/development-tool/atavrisp2){:target="_blank"}.
Honnêtement, je trouve que le coût de ce programmateur n’est pas si excessif que ça, il est
automatiquement reconnu par Atmel Studio, et permet de programmer tous les AVR disposant
d'une interface ISP (donc tous les AVR d’Atmel...).

Voilà, avec tout cela, je peux démarrer un premier projet très simple : Faire clignoter une LED !
