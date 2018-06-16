# Contexte

[//]: # --> Autre entreprise (cf Nicolas/INOVELAN?)

## Contexte général

Ce rapport est tiré de mon projet de fin d'étude à Polytech Lille.
Il s'agit d'un projet de recherche qui s'inscrit dans le contexte d'une collaboration entre l'équipe RMoD d'Inria Lille Nord Europe et Berger-Levrault.

Berger-Levrault invente et développe des solutions pour les administrations et les collectivités locales, pour les établissements d’éducation et de santé publics comme privés, les universités et les entreprises.
L'entreprise est implantée en France, au Canada et en Espagne.

## Problématique

Berger-Levrault possède des applications client/serveur qu’elle souhaite rajeunir.
En particulier, le front-end est développé en GWT et doit migrer vers Angular 4.
Le back-end est une application monolithique et doit évoluer vers une architecture de services Web.
Le changement de framework[^framework] graphique est imposé par l'arrêt du développement de GWT
  par Google et le problème de compatibilité arrière entre Angular 4 et Angular 3.
Le passage à une architecture à services est aussi souhaité pour améliorer l’offre commerciale et la rendre plus flexible.

Pour ce projet de fin d'études, je n'ai dû traiter que le passage de GWT à Angular.

## Description du problème

Les applications front-end de Berger-Levrault sont développées en Java en utilisant le framework GWT de Google.
Dans l'optique d'homogénéiser le visuel de leurs applications, Berger-Levrault a étendu ce framework.
Cette extension s'appelle **BLCore**.
Les applications de Berger-Levrault utilisent et/ou étendent **BLCore**, qui lui même utilise et/ou étend **GWT** comme présenté Figure \ref{architectureBL}.

![Structure application](figures/Modele BL App - Structure.png){#architectureBL width=250px height=250px}

Les applications de Berger-Levrault sont complexes.
Ce sont les plus importantes application GWT en terme de ligne de code et de classes dans le monde.
Elles définissent plusieurs centaines de pages web.
Bien qu'une migration complète de l'application en réécrivant l'ensemble du code est possible,
  c'est une tâche couteuse et sujette à erreurs.
Automatiser tout ou partie de la migration semble donc être la bonne solution, cependant les développeurs
  ne seront pas formés sur la nouvelle technologie et sur son utilisation dans les nouvelles applications.
Une alternative pour contourner ce problème serait de créer des outils facilitant la migration.
Les développeurs pourront alors effectuer la migration rapidement et
  seront formés sur le nouveau langage et sur l'application

La complexité de la migration des applications de Berger-Levrault réside dans
  leur taille mais aussi et surtout dans le changement de langage.
En effet, les applications sont développées intégralement en Java tout comme
  le framework GWT.
Or, pour utiliser Angular 4, le programme doit être écrit en TypeScript.
La question qui se pose est de savoir si nous pouvons garder le code source Java et, si c'est le cas, celui correspondant à quelles couches de la Figure \ref{architectureBL} ?

J'ai trouvé trois manières d'effectuer la migration :

- Réécrire l'ensemble des applications de Berger-Levrault pour qu'elles utilisent directement du TypeScript.
  Les problèmes ici sont qu'il existe beaucoup d'applications et que le code métier est écrit en Java.
  Il faudra donc de toutes façon faire co-exister Java et TypeScript.
  De plus, même si cette solution est choisie, il faudra aider les développeurs à migrer le code Java
    des parties graphiques en code TypeScript de façon semi-automatisée pour faciliter la migration.
- Migrer la couche BLCore puis migrer les applications en utilisant cette couche.
  Ce sera alors la couche BLCore qui fera le lien entre Java et TypeScript.
- Continuer à développer en Java, et modifier le générateur de GWT pour en créer un qui
  génère du TypeScript plutôt que du HTML, CSS et Javascript.

Mon objectif est donc d'évaluer la complexité de chacune de ces solutions.
Pour cela, j'ai dans un premier temps étudié la structure d'une application de Berger-Levrault.
Puis, j'ai voulu estimer la complexité de migrer la couche BLCore.
Caractériser les liens entre deux frameworks est alors une étape qui pourrait aider
  les développeurs dans leur travail.
C'est ce que j'appelle l'étude de l'adhérence entre deux frameworks.
Quelles classes je dois migrer ? Par laquelle je dois commencer ? Sont les questions
  auxquelles j'ai essayé de répondre.

J'ai effectué cette étude sur l'application *bac-à-sable* de Berger-Levrault.
Cette dernière permet aux employés de Berger-Levrault de consulter les
  éléments disponibles depuis BLCore.
Bien que plus petite que les applications en production,
  elle contient plusieurs centaines de classes.

[^framework]: Framework: ensemble cohérent de composants logiciels structurels, qui sert à créer les fondations ainsi que les grandes lignes de tout ou d’une partie d'un logiciel (architecture).

\newpage
