---
layout: post
author: zcool
title:  "Astuce C# du jour : Déplacer les DLL d'un binaire DotNet"
categories: [Développement, CSharp]
---

En temps normal, sans parler du GAC (Global Assembly Cache), toutes les DLL dont
dépendent un binaire C# doivent se trouver dans le même répertoire que ce dernier
pour qu'il puisse s'exécuter correctement… Malheureusement, plus les projets grossissent,
et plus le nombre de DLL augmente. Ce qui rend l'organisation du répertoire du binaire
quelque peu anarchique !

Et bien, voici la solution miracle : Il est possible d'indiquer par configuration les
répertoires potentiels où se trouvent les DLL du projet ! :grin:

Prenons l'exemple d'une application quelconque "CanShooter.exe" qui a besoin de la
DLL "CanShooter.Bean.dll" pour fonctionner. Si nous déplaçons la DLL dans un
sous-répertoire "Custom API", l'application ne fonctionne plus !

La magie opère lorsque l'on créé un fichier de configuration de l'application nommé
"CanShooter.config" et qui contient les éléments suivants :

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
 <runtime>
   <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
     <probing privatePath="Custom API"/>
   </assemblyBinding>
 </runtime>
</configuration>
```

Et voilà, le tour est joué ! :grin: