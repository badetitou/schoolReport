# Contexte

## Contexte général

Mon stage en entreprise est un travail qui s'inscrit dans le contexte d'une collaboration entre l'équipe RMoD d'Inria Lille Nord Europe et Berger-Levrault.

Berger-Levrault invente et développe des solutions pour les administrations et les collectivités locales, pour les établissements d’éducation et de santé publics comme privés, les universités et les entreprises.
L'entreprise est implantée en France, au Canada et en Espagne.

J'ai travaillé dans l'équipe recherche et développement de Berger-Levrault à Montpellier.
Mes superviseurs entreprises étaient M. Laurent Deruelle et M. Abderrahmane Seriai.
Ma tutrice école était Mme Anne Etien.
J'ai, dans le cadre de la collaboration entre Berger-Levrault et l'Inria Lille Nord Europe, travaillé aussi avec M. Nicolas Anquetil.

Ce travail est la suite du travail préliminaire que j'ai mené pendant mon Projet de Fin d'Étude à Polytech Lille.

## Problématique

Berger-Levrault possède des applications client/serveur qu’elle souhaite rajeunir.
En particulier, le front-end est développé en GWT et doit migrer vers Angular 6.
Le back-end est une application monolithique et doit évoluer vers une architecture de services Web.
Le changement de framework[^framework] graphique est imposé par l'arrêt du développement de GWT
  par Google et le problème de compatibilité arrière entre Angular 6 et Angular 1 (AngularJS).
Le passage à une architecture à services est aussi souhaité pour améliorer l’offre commerciale et la rendre plus flexible.

Mon travail durant ce stage ne traite que de la migration des applications front-end.

[^framework]: Framework: ensemble cohérent de composants logiciels structurels, qui sert à créer les fondations ainsi que les grandes lignes de tout ou d’une partie d'un logiciel (architecture).

## Description du problème

Les applications front-end de Berger-Levrault sont développées en Java en utilisant le framework GWT de Google.
Dans l'optique d'homogénéiser le visuel de leurs applications, Berger-Levrault a étendu ce framework.
Cette extension s'appelle **BLCore**.
La Figure \ref{architectureBL} représente les différentes couches de framework utilisé par une application de Berger-Levrault.
Les applications utilisent et/ou étendent **BLCore**, qui lui même utilise et/ou étend **GWT**.
On retrouve aussi des applications, qui ne respectent pas la convention décidé par les équipes de Berger-Levrault,
    ayant un lien direct avec le framework **GWT**.

![Structure application](figures/structure.png){#architectureBL width=250px height=250px}

Les applications de Berger-Levrault sont complexes.
Ce sont les plus importantes applications GWT en terme de ligne de code et de classes dans le monde.
Elles définissent plusieurs centaines de pages web.
Bien qu'une migration complète de l'application en réécrivant l'ensemble du code est possible,
    c'est une tâche coûteuse et sujette à erreurs.
Automatiser toute la migration semble donc être la bonne solution, cependant les développeurs
    ne seront pas formés sur la nouvelle technologie et sur son utilisation dans les nouvelles applications.
Le manque de connaissance du langage cible va ralentir les développements et peut faire _peur_ aux développeurs
    qui pourrait refuser une telle solution.
Une alternative pour contourner ce problème serait de créer des outils facilitant la migration et en migrer une partie de l'application.
Les développeurs pourront alors effectuer la fin de la migration rapidement et seront formés sur le nouveau langage et sur l'application en le pratiquant assisté de nos outils.

La complexité de la migration des applications de Berger-Levrault réside dans
  leurs tailles mais aussi et surtout dans le changement de langage.
En effet, les applications sont développées intégralement en Java tout comme le framework GWT.
Or, pour utiliser Angular 6, le programme doit être écrit en TypeScript.
La question qui se pose est de savoir quelle couche de la Figure \ref{architectureBL} nous devons migrer et comment ?

En plus des difficultés techniques inherentes à un tel projet, une entreprise comme Berger-Levrault a aussi
    des contraintes provenant de leurs développeurs et de leurs clients.
Parmi ces contraintes, la migration devra, entre autre,
    conserver l'architecture sur laquelle se base les applications de Berger-Levrault et
    ne pas perturber les clients de Berger-Levrault du point de vue visuel des applications et comportemental.

Mon objectif est donc de trouver des solutions pour aider à la migration des applications de Berger-Levrault,
    de les évaluer et d'implémenter la meilleure solution respectant les contraintes de l'entreprise.
Pour cela, j'ai dans un premier temps étudié la structure d'une application de Berger-Levrault.
Puis, j'ai défini avec un expert Angular l'architecture attendu pour les applications post-migration.
Ensuite, j'ai définie une stratégie pour faire la migration en m'inspirant d'une étude de l'état de l'art que j'ai menée.
Enfin, j'ai commencé le développement d'une suite d'outil permettant de mettre en application
    la stratégie définie et l'évaluer.

J'ai effectué cette étude sur l'application *bac-à-sable* de Berger-Levrault.
Cette dernière permet aux employés de Berger-Levrault de consulter les éléments disponibles depuis BLCore.
Bien que plus petite que les applications en production, elle contient tout de même plusieurs centaines de classes.
Cependant, j'ai régulièrement vérifier que mon travail pouvait s'appliquer sur les projets plus important de Berger-Levrault.

\newpage
