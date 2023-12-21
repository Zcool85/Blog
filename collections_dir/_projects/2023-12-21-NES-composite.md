---
title:  "NES composite avec sortie son stéréo"
type: Consoles
last_modified_at: 2023-12-21 21:55:00 +0100
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

> A tester : [Autre circuit composite](https://ctrl-alt-rees.com/2019-01-26-nintendo-famicom-composite-video-output-mod.html)
{: .prompt-info }