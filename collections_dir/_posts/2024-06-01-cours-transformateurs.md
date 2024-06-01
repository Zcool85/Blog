---
layout: post
author: zcool
title:  "Les transformateurs"
categories: [Electronique, Cours]
math: true
---

Il existe deux types de transformateurs :

- Le transformateur de châssis ou transformateur à étriers
- Le transformateur Torique

Si tension au secondaire inférieure au primaire, on parle alors de transformateur abaisseur de tension.

Si tension au secondaire supérieure au primaire, on parle alors de transformateur élévateur de tension.

La fréquence au secondaire est identique à la fréquence au primaire.

La tension est égale au rapport du nombre de spires des deux enroulements. Exemple : Pour un primaire de 1000 spires $S_{p}$, un secondaire de 52 spires $S_{s}$, et une tension au primaire $V_{p}$ de 230 Volts, la tension au secondaire $V_{s}$ sera égale à :

$$\begin{aligned}
  V_{s} = \frac{V_{p}}{\frac{S_{p}}{S_{s}}} & \Rightarrow V_{s} = \frac{230}{\frac{1\,000}{52}} \\
  & \Rightarrow V_{s} = 11.96V
  \end{aligned}$$

Pour ce qui est du courant transmis par induction, il est égal au rapport $\frac{V_{p}}{V_{s}} = \frac{230}{11.96} = 19.23$ (en négligeant les pertes).

Il est alors possible de déterminer l'intensité au primaire. Exemple : Pour $5A$ consommés au secondaire, l'intensité au primaire sera $\frac{5}{19.23} = 0.26A$.

Le courant que peut débiter un transformateur est calculé à partir de sa puissance mesurée en Volt Ampères ($VA$).

Quelques exemple :

- Pour un transformateur dont le primaire est à $230V$ avec un secondaire à $12V$ d'une puissance de $2VA$, alors la courant maximum au secondaire $I_{sec} = \frac{2VA}{12V} = 0.17A$
- Pour un transformateur dont le primaire est à $230V$ avec 2 secondaires de $18V$ chacun et d'une puissance totale de $160VA$ ($80VA$ par secondaire), alors le courant maximum pour un secondaire est $I_{sec1} = \frac{80VA}{18V} = 4.44A$ (Idem pour $I_{sec2}$)

Les pertes d'induction sont appelées coefficient de couplage.

Les deux enroulements n'ont aucun lien entre eux, c'est ce que l'on appel l'isolation galvanique.

Il peut y avoir plusieurs enroulements primaires et/ou secondaires. Ils peuvent être câblés en série (permet d'augmenter la tension) ou en parallèle (permet d'augmenter la puissance). Quelques exemples :

![Un primaire, Un secondaire](/assets/posts/learning-transfo/prim-sec.png){: width="400" }
_Fig. 1 : Un primaire, Un secondaire_

![Transformateur avec deux entrées](/assets/posts/learning-transfo/2prim-sec.png){: width="400" }
_Fig. 2 : Un primaire avec deux entrées (par exemple pour une entrée en $110V$ ou $230V$)_

![Un primaire, deux secondaires en série](/assets/posts/learning-transfo/prim-2sec-serie.png){: width="400" }
_Fig. 3 : Un secondaire à point milieu. Par exemple un $2 \times 15V$, on a $30V$ entre A et C_

![Un primaire et deux enroulements secondaires](/assets/posts/learning-transfo/prim-2sec-enrol.png){: width="400" }
_Fig. 4 : Deux enroulements secondaires_

Avec un transformateur à deux enroulements secondaires (Fig. 4), on peut obtenir différentes tensions et différentes intensités, selon qu'ils soient câblés en série ou en parallèle.

Prenons l'exemple d'un transformateur dont les caractérisques sont les suivantes :

- $2 \times 15V$
- $220VA$
- La tension entre A et B est de $15 V$ pour $110VA$. Idem pour la tension entre C et D.

Voici les deux montages possibles avec ce transformateur :

![Branchement en série](/assets/posts/learning-transfo/prim-2sec-enrol-serie.png){: width="400" }
_Branchement en série_

Branchement en série : Si l'on relie B et C, on obtient le transformateur Fig. 3. C'est-à-dire $30 V$ entre A et D pour $220 VA$; B et C pouvant servir de point milieu.

![Branchement en parallele](/assets/posts/learning-transfo/prim-2sec-enrol-para.png){: width="400" }
_Branchement en parallele_

Branchements en parallèle : Si l'on relie A à C et B à D, alors la tension entre A et D (ou B et C) est de $15 V$ pour $220 VA$ (les deux secondaires sont alors en phase).

> Pour reconnaitre le primaire du secondaire, il suffit d'utiliser un ohmmètre. L'enroulement présentant la résistance la plus faible est le secondaire.
{: .prompt-tip }

> Il est facile de calculer le courant au primaire en fonction de la consommation du courant au secondaire et donc d'en déduire la valeur minimale d'un fusible de protection du primaire (utiliser un fusible temporisé). Par exemple, si le circuit au secondaire consomme 1 A, pour un transformateur de 230 Volts au primaire et 9 Volts au secondaire, alors le courant au primaire sera Iprim = 0.3A / (230V / 9V) = 0,039 A.
{: .prompt-tip }

A savoir :

- Un transformateur ne fonctionne qu'en alternatif
- La tension indiquée sur le transformateur est la tension obtenue en charge (C'est à dire qu'il débite son courant maximum). A vide (secondaire en l'air) la tension de sortie est supérieure d'environ 15%.
- Un transformateur chauffe peu. S'il devient brûlant, c'est qu'il est aux limites de sa puissance. S'il chauffe à vide, c'est qu'il est de mauvaise qualité.
- Un transformateur (surtout ceux à étriers) rayonnent le 50 Hz du secteur. Particulièrement gênant pour les chaînes Hi Fi.

Site Français constructeur de transformateurs en tout genres : [rah indel](https://www.rah-indel.fr).
