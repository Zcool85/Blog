---
layout: post
author: zcool
title:  "Créer un support d’installation OS X sur clef USB"
categories: [Informatique, MacOS X]
---

Si comme moi vous préférez conserver un support d’installation de vos systèmes OS X, voici
une petite astuce permettant de créer un support d’installation de Mac OS X sur une clef USB.

Il est possible, quelle que soit la version de Mac OS X, de créer une clef USB contenant le
logicielle d’installation. Pour se faire :

- Il faut disposer d’une clef USB avec suffisamment d’espace (8 Go minimum).
- Il faut télécharger l’installation de Mac OS X sur le Mac App Store. Ce logicielle
  d'installation se comporte comme une application classique, sauf que si vous lancer
  l'installation, cette application se supprimera à la fin de l'installation. Il faudra alors
  re-télécharger le logiciel depuis le Mac App Store. L'application se nomme par exemple
  "Install OS X El Capitan.app" pour El Capitan, ou "Install OS X Yosemite.app" pour Yosemite, etc.

Une fois ces deux pré-requis passé, la création de la clef USB d’installation se fait en deux étapes.

La première étape consiste à formater la clef USB avec le système de fichier
"Mac OS étendu (journalisé)" :

- Brancher votre clef USB sur le Mac
- Lancer l’outil "Utilitaire de disque" situé dans le répertoire "Utilitaires" du répertoire
  "Applications"
- Sélectionner la clef USB dans la liste des périphériques et cliquer sur "Effacer". L'outil va alors
  vous demander comment formater la clef USB. Laisser le nom "Sans titre", sélectionner le format
  "OS X étendu (journalisé)", sélectionner le Schéma "Table de partition GUID" et cliquer sur le
  bouton "Effacer". L'opération prend quelques instants à l'issues desquels vous trouverez sur
  le bureau une clef USB nommée "Sans titre".

La seconde étape consiste à placer le logiciel d'installation de Mac OS sur la clef USB. Pour
se faire, il faut utiliser une ligne de commande via l'outil "Terminal" (situé dans le
répertoire "Utilitaires" du répertoire "Applications") :

```bash
sudo /Applications/Install\ OS\ X\ El\ Capitan.app/Contents/Resources/createinstallmedia --volume /Volumes/Sans\ titre --application
path /Applications/Install\ OS\ X\ El\ Capitan.app --nointeraction
```

Bien entendu, il faut remplacer le nom "Installation OS X El Capitan.app" par le nom du logiciel
d'installation téléchargé. Le nom "Sans titre" doit également être modifié si vous n'avez pas nommé
la clef USB "Sans titre" lors de son formatage.

A noter que la commande "sudo" demande les droits administrateur sur la machine. Elle vous
demandera donc sans doute de saisir le mot de passe de l’administrateur.

L’opération peut prendre plusieurs dizaines de minutes en fonction de la vitesse de votre
disque et de votre clef USB.

A la fin de l'opération, votre clef USB doit s'être renommée "Install OS X El Capitan" (ou
"Install OS X Yosemite" en fonction du logiciel d'installation utilisé).

Une dernière chose, pour installer Mac OS X depuis la clef USB, il suffit de brancher la clef USB
et de relancer le Mac en maintenant la touche Option enfoncée jusqu'au choix du disque à utiliser
pour démarrer.
