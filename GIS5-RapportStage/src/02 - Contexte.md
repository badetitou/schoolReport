# Contexte

## Contexte général

Mon stage en entreprise est un travail qui s'inscrit dans le contexte d'une collaboration entre l'équipe RMod d'Inria Lille - Nord Europe et Berger-Levrault.

Berger-Levrault invente et développe des solutions pour les administrations et les collectivités locales, pour les établissements d’éducation et de santé publics comme privés, les universités et les entreprises.
L'entreprise est implantée en France, au Canada et en Espagne.

J'ai travaillé dans l'équipe recherche et développement de Berger-Levrault à Montpellier.
Mes superviseurs entreprises étaient M. Laurent Deruelle et M. Abderrahmane Seriai.
Ma tutrice-école était Mme Anne Etien.
J'ai, dans le cadre de la collaboration entre Berger-Levrault et l'Inria Lille - Nord Europe, travaillé aussi avec M. Nicolas Anquetil.

Ce travail est la suite du travail préliminaire que j'ai mené pendant mon Projet de Fin d'Études à Polytech Lille.

## Problématique

Berger-Levrault possède des applications client/serveur qu’elle souhaite rajeunir.
En particulier, le _front-end_ est développé en GWT et doit migrer vers Angular 6.
Le _back-end_ est une application monolithique et doit évoluer vers une architecture de services web.
Le changement de _framework_[^framework] graphique est imposé par l'arrêt du développement de GWT par Google.
Google prévoit tout de même de créer un transpilateur pour convertir le code GWT en Angular,
    cependant il n'y a pas de communication sur ce projet et l'entreprise ne peut pas se permettre de
    rester "bloqué" avec des technologies vieillissantes.
Le passage à une architecture à services est aussi souhaité pour améliorer l’offre commerciale et la rendre plus flexible.

Mon travail durant ce stage ne traite que de la migration des applications _front-end_.

[^framework]: _Framework_: ensemble cohérent de composants logiciels structurels, qui sert à créer les fondations ainsi que les grandes lignes de tout ou d’une partie d'un logiciel (architecture).

## Description du problème

![Structure application](figures/structure.png){#fig:architectureBL width=50%}

\bvc{Présentation général appli BL}
Les applications _front-end_ de Berger-Levrault sont développées en Java en utilisant le _framework_ GWT de Google.
Ce sont les plus importantes applications GWT en termes de ligne de code et de classes dans le monde.
Elles définissent plusieurs centaines de pages web.

\bvc{Présentation du framework BLCore}
Dans l'optique d'homogénéiser le visuel de leurs applications, Berger-Levrault a étendu GWT.
Cette extension, qui s'appelle BLCore, permet à Berger-Levraut de définir des composants qui seront communs à tous
    leurs développements.

\bvc{Introduction utilisation du framework}
La taille des applications ainsi que l'architecture des applications de Berger-Levrault les rendent complexes
La \figref{architectureBL} représente les différentes couches de _framework_ utilisées par les applications de Berger-Levrault.
Toutes les applications fonctionnent de manière indépendantes.
Comme le représente les flèches, les applications utilisent BLCore, qui lui-même utilise GWT.
On retrouve aussi des applications, qui ne respectent pas la convention décidée par les équipes de Berger-Levrault,
    ayant un lien direct avec le _framework_ GWT.
C'est le cas de l'Application 1 qui utilise directement des composants de GWT.

\bvc{complexité de la migration}
En plus de l'architecture des applications à étudier,
    la complexité de la migration des applications de Berger-Levrault réside dans
    leurs tailles, mais aussi et surtout dans le changement de langage d'implémentation.
En effet, les applications actuelles et les _frameworks_ BLCore et GWT sont développées intégralement en Java.
Or, pour utiliser Angular 6, l'application doit être écrit en TypeScript.
La question qui se pose est de savoir quelle couche, de la \figref{architectureBL}, nous devons migrer et comment ?

\bvc{compléxité lié à BL}
De plus une entreprise comme Berger-Levrault a aussi
    des contraintes provenant de leurs développeurs et de leurs clients.
Parmi ces contraintes, la migration ne devra pas perturber les clients de Berger-Levrault du point de vue visuel et comportemental.

\bvc{proposition de solution}
Bien qu'une migration complète de l'application en réécrivant l'ensemble du code soit possible,
    ce serait une tâche coûteuse et sujette à erreurs.
Automatiser toute la migration semble donc être la bonne solution, cependant les développeurs
    ne seront pas formés sur la nouvelle technologie et sur son utilisation dans les nouvelles applications.
Le manque de connaissance du langage cible va ralentir les développements et peut faire peur aux développeurs
    qui pourraient refuser une telle solution.
Une alternative pour contourner ce problème serait de créer des outils facilitant la migration et de ne migrer qu'une partie de l'application.
Les développeurs pourront alors effectuer la fin de la migration assistée par l'outil ce qui les formera sur le nouveau langage et sur l'application.

\bvc{plan}
Notre objectif est donc de trouver des solutions pour aider à la migration des applications de Berger-Levrault,
    de les évaluer et d'implémenter la meilleure solution.
Pour cela, nous avons dans un premier temps étudié les contraintes de Berger-Levrault présenté \secref{descProbleme}.
\secref{stateOfTheArt}, nous avons étudié les techniques utilisées dans la littérature pour représenter une interface graphique.
Puis, \secref{guiDecomposition}, nous avons étudié les différentes composantes d'une application graphique.
Ensuite nous avons crée une approche et mis en place un outil, présenté \secref{secImplementation}, pour expérimenter la migration d'une application de Berger-Levrault.
Enfin, \secref{resultat}, \secref{discussion} et \secref{futurWork} nous avons discuté des résultats obtenues avec notre prototype et nous présentons
    les améliorations que nous devons encore apporter au travail effectué.

\bvc{desc rapide bac à sable}
J'ai effectué cette étude sur l'application _bac à sable_ de Berger-Levrault.
Cette dernière permet aux employés de Berger-Levrault de consulter les éléments disponibles dans BLCore.
C'est une application complète qui fonctionne exactement comme une destinée aux clients de Berger-Levrault.
En raison de son statut d'application modèle pour les développeurs,
    elle contient tout ce qui est utilisable dans les autres applications.
En plus des outils utilisable par les développeurs,
    l'application est aussi composé de code déviant des règles de programmation définit par Berger-Levrault.

Bien que plus petite que les applications en production, elle contient tout de même plus de 1 000 classes Java.
Le _bac à sable_ définit plus de 50 pages web
    qui peuvent être simples ou complexes (_c.-à-d._ qui comporte des formulaires, effectue des requêtes sur des serveurs distants, etc.).

\bvc{explication travail pour comprendre}
Nous avons aussi régulièrement vérifié que notre travail pouvait s'appliquer sur les projets plus importants de Berger-Levrault.
Pour cela, nous avons utilisé notre prototype sur l'application _RH_ de Berger-Levrault.
L'application _RH_ est destiné aux clients désirant centralisé la gestion du personnel
    de leurs compagnie dans une solution logiciel sur le web.
Elle est constitué de plus 450 pages web et de 19 000 classes Java.
