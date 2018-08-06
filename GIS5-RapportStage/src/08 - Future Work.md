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
Pour le moment, le layout est considéré comme un Attribut, il nous est difficile de représenter des interfaces complexes.
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
Cette amélioration devrait nous permettre de mieux représenter les comportements de l'application
    lorsqu'un utilisateur effectue une action sur l'interface.

Enfin, il nous faut concevoir un méta-modèle pour le code métier.
Cela devrait permettre d'extraire et représenter les règles métiers d'une application.

## Outil de validation

Un des problèmes que nous avons actuellement est l'évaluation de la migration.
Il est facile de prendre l'application source et l'application cible et de regarder _manuellement_
    si la migration a bien été effectué, mais sur des applications de la taille de celle de Berger-Levrault
    il faudrait un système automatique.
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
Pour comparer les images, il est possible d'utiliser des solutions de comparaison pixel par pixel
    ou d'utiliser une intelligence artificielle qui reconnaît les images similaires.

Pour la validation des règles métiers et l'application du code comportemental,
    il est possible d'utiliser les tests utilisés en recette à Berger-Levrault.

Il est possible que les tests automatiques ne fonctionnent plus après la migration,
    en effet, ils peuvent être basés sur des méta-données que nous modifions pendant la migration, par exemple pour des raisons de compatibilité dans le langage cible.
Il nous faudra alors effectuer la migration des tests d'intégration de Berger-Levrault pour les rendre compatible avec l'application migrée.

## Complétude du travail

Le travail que nous avons mené nous a permis de migrer des pages web de l'application _bac-à-sable_.
Cependant, comme présenté Section \ref{retroInge}, nous n'arrivons pas à migrer 100% des widgets.
De plus, à cause du problème d'outil de validation, il est difficile de savoir si la migration s'effectue complètement,
    et correctement.

Comme nous avons vérifié nos travaux avec les même page web qui nous ont permis de créer les outils de migration,
    nous n'avons pas connaissance de potentiel erreurs, ou oublis, qui rendrez l'outil moins performants.
Afin de créer un outil complet, nous devrions chercher et gérer tous les écarts de programmation potentiels pour l'importer.

De même, nous savons que nous n'avons pas géré le cas des interfaces graphiques définis via fichier xml.
Bien que cela ne semble pas bloquant dans le cas des projet de Berger-Levrault car cette solution n'est utilisé
    que dans l'application _bac-à-sable_.
Il serait intéressant de voir s'il est _"facile"_ d'ajouter cette fonctionnalité.
En particulier, l'ajout de cette fonctionnalité permettrait d'analyser l'extensibilité de l'importer.

## Exportation du Core

La stratégie que nous avons créer permet d'effectuer la migration d'interface graphique d'un langage source vers un langage cible.
Entre autre, il est possible de configurer l'outil pour que l'application cible utilise tel ou tel autre framework
    pour représenter les éléments graphique de l'interface final.

Dans le cas d'une application Web, un certains nombre de widget sont déjà pré-existants.
Pour chacun d'entre eux nous retrouvons un tag HTML.

Lors de la migration, l'utilisation de ces widgets nous permet d'avoir rapidement un retour visuel.
Mais sans l'exportation du style précis des widgets, il est impossible de respecter la contrainte
    de _préservation du visuel_.

De plus, il peut exister des widgets définis dans le langage source qui n'existe pas, ou partiellement, dans le langage cible.
Dans ce cas, il faut soit accepter une différence entre les deux interfaces, soient créer ou utilisé un autre framework dans le langage cible.

Pour la migration que veut mettre en place Berger-Levrault, le framework utilisé dans le langage source est BLCore.
Comme il n'existe pas de framework BLCore utilisable en Angular, nous avons utilisé le framework PrimeNG qui nous
    permet de facilement retrouver la majorité des widgets décrit dans BLCore.
Il reste cependant des écarts visuel entre les deux framework et certains widget, comme la BLTableBulk qui est un widget abondamment utilisé dans les applications de l'entreprise,
    n'a pas de correspondance.

Afin de respecter la contrainte de _préservation du visuel_, une solution est donc de créer un équivalent du framework BLCore de Java en Angular.
La migration d'un framework consiste en la détection des différents widgets qu'il définit,
    leurs compositions, et le style qui leurs sont attachés.
Les widgets peuvent aussi contenir du comportement qu'il faudra détecter en migrer.

Un travail futur serait de développer un outil qui permettrai de détecter tous ces composants d'un widget et ainsi de faciliter leurs migrations.
En fonctionnant en symbiose avec l'outil que nous avons créé pendant ce stage, il serait ainsi possible d'effectuer la migration d'une application graphique
    en respectant la hiérarchie des widgets, les contraintes de layout et le visuel et comportement des widgets.

\newpage