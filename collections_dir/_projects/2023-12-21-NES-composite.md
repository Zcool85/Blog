---
title:  "NES composite avec sortie son stéréo"
type: Consoles
last_modified_at: 2024-02-17 14:35:00 +0100
state: En cours
---

Le PPU de la NES sort un format natif composite. Toutefois, en France, les consoles NES
ont un module permettant de transformer le signal composite en signal RGB. Mais ce module
a malheureusement tendance à dégrader l'image. C'est pourquoi j'ai décidé de remplacer le
module par un fait maison qui me permettra de restituer le signal composite sortant du PPU.

<!--more-->

> De plus, j'en profite pour convertir le son mono en "stéréo"
{: .prompt-info }

Dépôt GitHub : [https://github.com/Zcool85/NESComposite](https://github.com/Zcool85/NESComposite){:target="_blank"}

## TODO

- [x] Faire le sous-projet "Composite video mod"
  + [x] Shéma de principe
  + [x] Documentation
  + [x] Tester la première version du [circuit composite](https://www.nesdev.org/wiki/PPU_pinout){:target="_blank"}
  + [x] Tester une autre version du [circuit composite](https://ctrl-alt-rees.com/2019-01-26-nintendo-famicom-composite-video-output-mod.html){:target="_blank"}
         Inspiré du site [nesdev](https://forums.nesdev.org/viewtopic.php?f=9&t=10554){:target="_blank"}
- [x] Faire le sous-projet "Audio mod"
  + [x] Shéma de principe
  + [x] Documentation
- [x] Faire le sous-projet "CIC mod"
  + [x] Documentation
- [ ] Faire le sous-projet "Remplacement RVB"
  + [x] Shéma de principe
  + [ ] PCB
  + [ ] Documentation
  + [ ] Finaliser le schéma avec [nesmod](http://rnc.free.fr/nesmod/){:target="_blank"}.


## Sortie vidéo composite

Le PPU de la console sort directement le signal vidéo composite depuis la broche N°21. Il est donc
possible de récupérer le signal directement depuis cette broche et d'amplifier le signal.

J'ai testé deux versions :

1. Une version issue de [nesdev](https://www.nesdev.org/wiki/PPU_pinout){:target="_blank"}

2. Une version issue d'un post sur [ctrl-alt-rees](https://ctrl-alt-rees.com/2019-01-26-nintendo-famicom-composite-video-output-mod.html){:target="_blank"}


Après avoir tester les deux versions, seule la première me donne satisfaction. Le signal est un peu
plus propre, sans être non plus exceptionnel.

Je retiens donc le schéma suivant :

![NES Composite](/assets/projects/NESComposite/Composite_video_mod.jpg)
_Schéma de principe pour la sortie composite NES_

![Broches PPU](/assets/projects/NESComposite/nes_ppu.jpeg)
_Emplacement des broches PPU_

## Audio "stéréo"

Pour l'audio stéréo, l'objectif consiste à mixer le canal audio N°1 du CPU avec le canal mono sur la voie de gauche et
mixer le canal audio N°2 du CPU avec le canal mono sur la voie de droite.

Le schéma suivant présente le mixing :

![Audio Stereo](/assets/projects/NESComposite/audio_stereo_mod.png)
_Schéma de principe pour la sortie audio stéréo_

> Les résistances variables permettent de mixer plus ou moins le canal mono sur chacune des voies.
{: .prompt-info }

![Broches audio du CPU](/assets/projects/NESComposite/audio_cpu.jpeg)
_Emplacement des broches audios sur le CPU_

![Flux audio mono](/assets/projects/NESComposite/audio_mono.jpeg)
_Emplacement de la broche du flux audio mono_

## Remplacement RVB

Dans ma toute première version du moding de ma NES, j'avais remplacé intégralement le module de convertion RGB
dans lequel j'avais intégré l'alimentation, la sortie composite et le son stéréo. Ce dernier nécessite la
modification de la prise péritel car il manque une broche pour l'audio gauche...

Le tableau suivant référence les 10 broches du connecteur standard NES vers SCART :

| Pin | Name    | Direction | Description             | Signal Level | Impédance
|----:|---------|-----------|-------------------------|--------------|----------
| 1   | BLNK    | OUT       | Fast blanking / Fast switch | 1~3 V => RGB<br />0~0.4 V => Composite | 75 ohm
| 2   | SWTCH   | OUT       | Slow switch (Video format) |              |
| 3   | R       | OUT       | Red                     | 0.7 V        | 75 ohm
| 4   | C_GND   | GND       | Composite Video Ground  |              |
| 5   | G       | OUT       | Green                   | 0.7 V        | 75 ohm
| 6   | R_G_GND | GND       | Red and Green ground    |              |
| 7   | B       | OUT       | Blue                    | 0.7 V        | 75 ohm
| 8   | B_A_GND | GND       | Blue and Audio ground   |              |
| 9   | C       | OUT       | Composite Video         | 1 V         | 75 ohm
| 10  | A       | OUT       | Audio mono              | 0.5 V rms    | < 1K ohm

Les dix broches sont bien entendu reliées au connecteur SCART. La seule façon de faire passer la stéréo
est donc d'utiliser une des pins RGB qui va disparaitre, mais celà nécessite également de modifier
le connecteur SCART... Et donc modifier le cablage d'origine. Pire encore, si l'on utilise un cable
officiel, alors un signal audio sera transmis sur un canal RGB ce qui n'est pas conseillé.

J'ai donc ici deux solutions :

- Soit je passe en mode "stéréo" et dans ce cas une modification péritel s'impose
- Soit j'abandonne le mode "stéréo" et je peux alors conserver la péritel standard

Pour cette nouvelle version, il n'y aura pas de mode "stéréo".
