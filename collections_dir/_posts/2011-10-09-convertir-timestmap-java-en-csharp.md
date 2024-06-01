---
layout: post
author: zcool
title:  "Convertir les TimeStamp Java en DateTime C#"
categories: [Développement, CSharp]
---

En bidouillant un peu les données récupérées par l'API Wow, je me suis rendu compte
que certaines données sont en fait des TimeStamp Java. Le TimeStamp est une unité
de mesure du temps utilisé principalement en Java.

Afin de pouvoir utiliser ces données dans mes traitements C#, j'ai fait quelques
recherches sur le sujet…

Le TimeStamp Java s'appuie sur une date de référence. Cette date est fixée
au 1 janvier 1970 à minuit. En java, lorsque l'on utilise un TimeStamp, il s'agit en
réalité du nombre de millième de secondes écoulés depuis la date de référence
(1 000 unités TimeSpamp donnent une seconde). En C# en revanche, l'unité de mesure
est le tick (1 seconde vaut 10 000 000 ticks sous Windows).
Cf. [https://msdn.microsoft.com/en-us/library/system.datetime.ticks.aspx](https://msdn.microsoft.com/en-us/library/system.datetime.ticks.aspx){:target="_blank"} pour le détail d'un DateTime C#.

Et bien maintenant c'est tout simple ! Ce n'est qu'un peu de math… Je passe les
détails du calcul, mais il suffit de multiplier par 10 000 le TimeStamp Java
pour obtenir un nombre de Ticks Windows. Une fois ce nombre calculé, je n'ai
plus qu'à l'ajouter à ma date de référence (01/01/1970) et le tour est joué !

Ah si, une dernière remarque : Il faut toujours penser aux fuseaux-horaires
quand on joue avec les dates… Donc pour obtenir la vraie date dans le fuseau-horaire
courant, il faut appliquer la fonction « ToLocaleTime ». Cette dernière se charge
d'appliquer les décalages horaires adéquat en fonction de la machine sur laquelle
s'exécute le traitement. Ainsi, plus besoin de s'enquiquiner avec l'ajout d'une ou 
deux heures…

Allez, un petit bout de code pour le fun :

```csharp
/// <summary>
/// Helper pour les méthodes de compatibilité Java
/// </summary>
public static class JavaHelper
{
    private static DateTime _referenceDate = new DateTime(1970, 1, 1, 0, 0, 0, DateTimeKind.Unspecified);
 
    /// <summary>
    /// Fonction permettant de convertir les TimeStamp Java en DateTime C#
    /// </summary>
    /// <param name="timeStamp">TimeStamp Java</param>
    /// <returns>Date C#</returns>
    public static DateTime ConvertJavaTimeStampToDateTime(long timeStamp)
    {
        return _referenceDate.AddTicks(timeStamp * 10000).ToLocalTime();
    }
}
```

NOTE : Pour le moment, la plus petite unité de mesure du temps que j'ai réussi à trouver
sur UNIX est le centième de seconde (0.01 sec). il faut utiliser la fonction POSIX `times`
définie dans l'include `sys/times.h`.