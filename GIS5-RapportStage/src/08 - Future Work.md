# Travaux futurs

Nous avons réussi à construire des bases solides pour l'outil de migration.
Afin de l'améliorer, il faudrait encore ajouter la gestion des layout, du code comportemental et du code métier.
Nous allons devoir corriger les quelques erreurs restantes qui nous empêchent d'importer 100 %
    des widgets que nous souhaitons exporter.
Enfin, nous devons définir une ou plusieurs stratégies pour évaluer correctement la complétude de notre travail.

## Amélioration des modèles

Comme expliqué Section \ref{exportToAngular}, certains résultats de l'exportation ne sont pas correct à cause d'un manque de respect des layouts.
C'est le cas de la Figure \ref{cmp2}.
Ceci est dû à la manière dont est retranscrite l'idée de layout dans le méta-modèle d'interface graphique.
Pour le moment, le layout est considéré comme un attribut, ce qui rend difficile la représentation d'interfaces complexes.
Plusieurs solutions peuvent être envisagées pour représenter les interfaces graphiques.

Gotti et Mbarki [@gotti2016java] ont décidé d'utiliser le méta-modèle proposé par KDM[^kdm].
Le méta-modèle KDM de layout utilise une association entre deux _AbstractUIElement_ pour représenter la position de l'un part rapport à l'autre.
C'est ainsi qu'est défini le layout d'une application.

[^kdm]: KDM : Knowledge Discovery Metamodel

Sánchez Ramón _et al._ [@sanchez2014model] ont créé un méta-modèle pour les layouts.
Ce méta-modèle propose de lier la notion de widget avec celle de layout et de combiner les layouts afin de
    créer un layout précis.
Les auteurs ont défini un sous-ensemble de layout qu'il est possible de connecter aux widgets.
Cette solution est celle qui se rapproche le plus des techniques de conception que nous utilisons.

Afin d'améliorer la représentation que nous avons d'une application graphique,
    il faudrait que l'on représente toutes les couches d'une interface graphique comme décrite Section \ref{guiDecomposition}.
Une fois le layout implémenté nous pourrons bien représenter l'interface utilisateur, il nous faudra encore définir et implémenter
    des méta-modèles pour le code comportemental et le code métier.

Nous avons déjà défini un méta-modèle de code comportemental qui est présenté Section \ref{metamodelComportemental}.
Il faut encore implémenter ce modèle et modifier en conséquence les importateur et exportateur que nous avons créé.
Cette amélioration devrait nous permettre de mieux représenter les comportements de l'application
    lorsqu'un utilisateur effectue une action sur l'interface.

Enfin, il nous faut concevoir un méta-modèle pour le code métier.
Cela devrait permettre d'extraire et représenter les règles métiers d'une application.

## Outil de validation

Un des problèmes que nous avons actuellement est l'évaluation de la migration.
Il est facile de prendre l'application source et l'application cible et de regarder _manuellement_
    si la migration a bien été effectuée, mais sur des applications de la taille de celle de Berger-Levrault
    nous avons besoin d'un système automatique.
Les éléments à évaluer sont les règles métiers dans l'application cible,
    le respect du visuel et l'application du code comportemental.

Pour le respect du visuel, plusieurs solutions peuvent être envisagées dans le cas de Berger-Levrault.

Il est possible de comparer les DOM des applications car nous travaillons avec des applications web.
Cependant le DOM ne prend en compte que la structure d'une page web et non pas sa représentation visuelle.
Une étude des fichiers CSS serait alors indispensable.
De plus, le fichier généré pour Angular n'est pas forcément une copie du fichier généré par GWT, il peut y avoir des différences dans le DOM
    qui ne sont pas répercuté dans l'aspect visuel de la page web.

Une autre solution est de comparer le visuel des pages web par comparaison d'image.
Il faudrait alors prendre une impression de chaque phase dans chacun de ses états,
    qui peut varier si l'on exécute du code comportemental,
    et la comparer avec l'impression de la page générée.
Pour comparer les images, il est possible d'utiliser des solutions de comparaison pixel par pixel
    ou d'utiliser une intelligence artificielle qui reconnaît les images similaires.

Pour la validation des règles métiers et l'application du code comportemental,
    il est possible d'utiliser les tests utilisés en recette à Berger-Levrault ou d'utiliser de la comparaison d'image.

Il est possible que les tests automatiques ne fonctionnent plus après la migration,
    en effet, ils peuvent être basés sur des métadonnées que nous modifions pendant la migration, par exemple pour des raisons de compatibilité dans le langage cible.
Il nous faudra alors effectuer la migration des tests d'intégration de Berger-Levrault pour les rendre compatibles avec l'application migrée.

Dans le cas d'un manque de tests pour une fonctionnalité,
    nous pourrions aussi créer des outils qui se basent sur les modèles de l'application à migrer pour
    générer des tests.
Ces outils assisteront les recetteurs de l'entreprise et permettront de tester l'outil de migration final.

Dans le cas de la comparaison d'image, nous pouvons nous inspirer des travaux de Joorabchi _et al._ [@joorabchi2012reverse] qui
    l'ont utilisé pour la génération des états pour leur méta-modèle de state flow.

## Complétude du travail

Le travail que nous avons mené nous a permis de migrer des pages web de l'application _bac à sable_.
Cependant, comme présenté Section \ref{retroInge}, nous n'arrivons pas à migrer 100 % des widgets.
De plus, à cause du problème d'outil de validation, il est difficile de savoir si la migration s'effectue complètement,
    et correctement.

Comme nous avons vérifié nos travaux avec les mêmes pages web qui nous ont permis de créer les outils de migration,
    nous n'avons pas connaissance de potentielles erreurs, ou oublis, qui peuvent rendre l'outil moins performant.
Afin de créer un outil complet, nous devrions chercher et gérer tous les écarts de programmation potentiels pour l'importateur.

De même, nous savons que nous n'avons pas géré le cas des interfaces graphiques défini via fichier XML.
Bien que cela ne semble pas bloquant dans le cas des projets de Berger-Levrault car cette solution n'est utilisée
    que dans l'application _bac à sable_.
Il serait intéressant de voir s'il est _"facile"_ d'ajouter cette fonctionnalité.
En particulier, cela permettrait d'analyser l'extensibilité de l'importateur.

## Exportation du Core

La stratégie que nous avons créée permet d'effectuer la migration d'interface graphique d'un langage source vers un langage cible.
De plus, il est possible de configurer l'outil pour que l'application cible utilise tel ou tel framework
    pour représenter les éléments graphiques de l'interface finale.

Dans le cas d'une application web, un certain nombre de widgets sont déjà préexistants.
Pour chacun d'entre eux, nous retrouvons un tag HTML.
Par exemple, il y a _input_, _div_, _a_ (pour un lien), etc.

Lors de la migration, l'utilisation de ces widgets nous permet d'avoir rapidement un retour visuel.
Mais sans l'exportation du style précis des widgets, il est impossible de respecter la contrainte
    de _préservation du visuel_.

De plus, il peut exister des widgets définis dans le langage source qui n'existe pas, ou partiellement, dans le langage cible.
Dans ce cas, il faut soit accepter une différence entre les deux interfaces, soient créer ou utilisé un autre framework dans le langage cible.

Pour la migration que veut mettre en place Berger-Levrault, le framework utilisé dans le langage source est BLCore.
Comme il n'existe pas de framework BLCore utilisable en Angular, nous avons utilisé le framework PrimeNG qui nous
    permet de facilement retrouver la majorité des widgets décrits dans BLCore.
Il reste cependant des écarts visuels entre les deux frameworks et certains widgets, comme la BLTableBulk qui est un widget abondamment utilisé dans les applications de l'entreprise,
    n'a pas de correspondance.

Afin de respecter la contrainte de _préservation du visuel_, une solution est donc de créer un équivalent du framework BLCore de Java en Angular.
La migration d'un framework consiste en la détection des différents widgets qu'il définit,
    avec leurs compositions, leurs styles et leurs code comportemental.

Un travail futur serait de développer un outil qui permettrait de détecter tous ces composants d'un widget et ainsi de faciliter leurs migrations.
En fonctionnant en symbiose avec l'outil que nous avons créé pendant ce stage, il serait ainsi possible d'effectuer la migration d'une application graphique
    en respectant la hiérarchie des widgets, les contraintes de layout et le visuel et comportement des widgets.
