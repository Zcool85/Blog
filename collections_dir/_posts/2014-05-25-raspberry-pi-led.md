---
layout: post
author: zcool
title:  "Faire clignoter une LED sur un raspberry pi"
categories: [Electronique, Raspberry pi]
---

Avoir un raspberry pi c'est bien... Le faire fonctionner c'est mieux ! :grin:

Comme première prise en main, je vous propose un petit montage simple permettant de faire
clignoter une LED avec le Raspberry pi.

Avant de commencer, quelques petites précautions s'imposent... Je ne veux pas cramer mon
raspberry ! :yum: Un petit tour sur les spécifications du GPIO nous apprend que :

- Les broches du GPIO délivrent un courant de 3,3V
- Une broche ne peut pas délivrer plus de 16 mA (intensité réglable programmatiquement
  entre 2 mA et 16 mA par pas de 2 mA)
- La broche P1 (3,3V) ne délivre pas plus de 50 mA
- De manière générale, la somme des intensités utilisés des broches du GPIO ne doit pas
  dépasser 50 mA.
- Il est possible d'utiliser la broche P2 (5V), mais dans ce cas, il faut que l'alimentation
  du raspberry délivre au moins 700 mA + la consommation du circuit branché sur la broche

Toute ces informations sont décrites sur [mosaic-industries](http://www.mosaic-industries.com/embedded-systems/microcontroller-projects/raspberry-pi/gpio-pin-electrical-specifications){:target="_blank"}.

Comme une broche du raspberry ne pourra pas délivrer plus de 16 mA, il est possible de brancher
en directe une LED sur la broche... Mais je ne le conseil pas ! Il est toujours préférable de
placer une résistance pour limiter l'intensité. Dans ce cas, il suffit d'utiliser une résistance
de 3.3V / 0.01 A = 330 ohms. La diode sera alimentée par un courant de 10 mA.

Maintenant, je vais vous proposer un petit montage simple permettant d'utiliser moins
d'1 mA sur une broche tout en pilotant l'allumage d'une diode. Pour ce faire, nous aurons
besoin d'un transistor (2N2222 – un classique :yum:) et d'une résistance de plus.

Voici le schéma (les explications sont incluses dans le schéma) :

![Schéma pour piloter une LED via une broche du GPIO](/assets/posts/RaspyLED.png)
_Schéma pour piloter une LED via une broche du GPIO_

Une fois le schéma réalisé, il ne reste plus qu'à créer un petit programme pour faire clignoter
la LED. Voici un exemple en python (La broche utilisée est la P11 nommée GPIO_17) :

```python
import RPi.GPIO as GPIO
import time

# blinking function
def blink(pin):
        GPIO.output(pin,GPIO.HIGH)
        time.sleep(1)
        GPIO.output(pin,GPIO.LOW)
        time.sleep(1)
        return

# to use Raspberry Pi board pin numbers
GPIO.setmode(GPIO.BOARD)
# set up GPIO output channel
GPIO.setup(11, GPIO.OUT)

# blink GPIO17 50 times
for i in range(0,50):
        blink(11)

GPIO.cleanup()
```
