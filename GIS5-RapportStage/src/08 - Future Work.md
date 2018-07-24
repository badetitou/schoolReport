# Travaux futurs

Nous avons réussi à construire des bases solides pour améliorer l'outil de migration que nous avons commencer.
Afin de l'améliorer, il faudrait encore ajouter la gestion des layout, du code comportemental et métier.
Nous allons devoir corriger les quelques erreurs restante qui nous empêche d'importer 100%
    des widgets que nous souhaitons exporter.
Enfin, nous devons définir une ou plusieurs stratégies pour évaluer correctement la complétude de notre travail.

## Amélioration des modèles

Comme expliqué Section \ref{exportToAngular}, certains résultats de l'exportation ne sont pas correct à cause d'un manque de respect des layouts.
C'est le cas de la Figure \ref{cmp2}.
Ceci est dû à la manière dont est retranscrit l'idée de layout dans le méta-modèle d'interface graphique.
Pour le moment, le layout est considéré comme un Attribut, il est donc difficile de représenter des comportements complexes.
Plusieurs solutions peuvent être envisagées pour représenter les interfaces graphiques.

Gotti et Mbarki [@gotti2016java] ont décidé d'utiliser le meta-modèle proposé par KDM [^kdm].
Le méta-modèle KDM de layout utilise une association entre deux _AbstractUIElement_ pour représenter la position de l'un part rapport à l'autre.
C'est ainsi qu'est défini le layout d'une application.

[^kdm]: KDM : Knowledge Discovery Metamodel

Sánchez Ramón _et al._ [@sanchez2014model] ont crée un méta-modèle pour les layouts.
Ce méta-modèle propose de lier la notion de widget avec celle de layout et de combiner les layouts afin de
    créer un layout précis.
Les auteurs ont défini un sous-ensemble de layout possible a connecter aux widgets.
Cette solution est celle qui se rapproche le plus des techniques de conception que nous utilisons.

Afin d'améliorer la représentation que nous avons d'une application graphique,
    il faudrait que l'on représente toutes les couches d'une interface graphique comment décrit Section \ref{guiDecomposition}.
Une fois le layout implémenté nous aurons bien représenter l'interface utilisateur, il nous faudra encore définir et implementer
    des méta-modèles pour le code comportemental et le code métier.

Nous avons déjà défini un méta-modèle de code comportemental qui est présenté Section \ref{metamodelComportemental}.
Il faut encore implémenter ce modèle et modifier en conséquence les importer et exporter que nous avons créé.
Cette amélioration devrait nous permettre de mieux représenter les comportement de l'application
    lorsqu'un utilisateur effectue une action sur l'interface.

Enfin, il nous faut concevoir un méta-modèle pour le code métier.
Cela devrait permettre d'extraire et représenter les règles métiers d'une application.

## Outil de validation

Un des problèmes que nous avons actuellement est l'évaluation de la migration.
Il est facile de prendre l'application source et l'application cible et de regarder manuellement
    si la migration a bien été effectué, mais sur des applications de la taille de celle de Berger-Levrault
    il faudrait un système plus automatique.
Les éléments à évaluer serait la validité des règles métiers dans l'application cible,
    le respect du visuel et l'application du code comportemental.

Pour le respect du visuel, plusieurs solutions peuvent être envisagés dans le cas de Berger-Levrault.

Il est possible de comparer les DOM des applications car nous travaillons avec des applications web.
Cependant le DOM ne prend en compte que l'architecture d'une page web et non pas ça représentation visuelle.
Une étude des fichiers CSS serait alors indispensable.
De plus, le fichier généré pour Angular n'est pas forcément une copie du fichier généré par GWT, il peut y avoir des différences dans le DOM
    qui ne sont pas répercuté dans l'aspect visuel de la page web.

Une autre solution serait de comparer le visuel des pages web par comparaison d'image.
Il faudrait alors prendre une impression de chaque Phase dans chacun de ses états,
    qui peut varier si l'on exécute du code comportemental,
    et la comparer avec l'impression de la page généré.
Pour comparer les images, il est possible d'utiliser des solutions de comparaison pixels par pixel
    ou d'utiliser une intelligence artificielle qui reconnaît les images similaires.

## Complétude du travail

## Exportation du Core

- missing widget

\newpage