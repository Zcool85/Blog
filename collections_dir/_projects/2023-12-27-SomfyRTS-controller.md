---
title:  "Controller de volet roulant Somfy RTS"
type: Domotique
last_modified_at: 2023-12-27 21:55:00 +0100
state: En cours
---

Mon objectif sur ce projet est de pouvoir controler tous mes volets roulant depuis
un seul et même appareil.

<!--more-->

Pour ce projet, j'ai décidé d'utiliser un ESP32 pour pouvoir envisager d'utiliser
mon réseau local pour, par la suite, pouvoir piloter les volets roulant.

Pour pouvoir communiquer avec les volets en RTS, j'ai décidé d'utiliser le module
RFM69HCW (module qui semble le plus simple à utiliser pour ce type de projet).

Bien entendu, ce projet me permettra de me familiariser avec les ESP32 et l'usage
de leur outil de développement ESP-IDF.

Pour faire ce projet, je part de ce site expliquant le [Protocole](https://pushstack.wordpress.com/somfy-rts-protocol/) Somfy RTS.

De plus, je m'insipre également de cette [bibliothèque C++](http://www.airspayce.com/mikem/arduino/RadioHead/index.html) pour coder une librairy dédiée au RFM69HCW.

Et enfin, je m'aide de ce [projet](https://github.com/etimou/SomfyRTS/tree/master) pour extraire le bon réglage du module.
