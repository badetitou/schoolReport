# Travaux futurs {#sec:futurWork}

Nous avons réussi à construire des bases solides pour l'outil de migration.
Afin de l'améliorer, il faudrait encore ajouter la gestion du code comportemental comme présenté \secref{futurComportementale}.
Nous allons aussi travailler sur la conception d'une approche pour évaluer
    les résultats que produit notre outil.

## Code comportemental {#sec:futurComportementale}

Nous avons présenté \secref{metamodelComportemental} et \secref{metamodelComportemental} un travail préliminaire
    sur la représentation du code comportemental.
Nous devons maintenant implémenter ce travail et le tester.
Cela devrait nous permettre d'améliorer la représentation du comportement des applications
    qui ont une interface graphique.

## Evaluation de la migration {#sec:validationFutur}

Nous n'avons pas trouvé de stratégie ou de métriques pour évaluer précisément la validité de
    notre prototype.
Il est donc important que l'on recherche des stratégies pour apprécier la qualité de notre approche.

L'évaluation devra nous permettre de vérifier l'aspect visuel, le code comportemental et le code métier
    de l'application à migrer ont bien été migré.

Pour l'aspect visuel, il est possible de comparer les DOM des applications car nous travaillons avec des applications web.
Cependant le DOM ne prend en compte que la structure d'une page web et non pas sa représentation visuelle.
Une étude des fichiers CSS serait alors indispensable.
De plus, le fichier généré pour Angular n'est pas forcément une copie du fichier généré par GWT, il peut y avoir des différences dans le DOM
    qui ne sont pas répercutées dans l'aspect visuel de la page web.

Une autre solution est de comparer le visuel des pages web par comparaison d'image.
Il faudrait alors prendre une impression de chaque phase dans chacun de ses états,
    qui peut varier si l'on exécute du code comportemental,
    et la comparer avec l'impression de la page générée.
Pour comparer les images, il est possible d'utiliser des solutions de comparaison pixel par pixel
    ou d'utiliser une intelligence artificielle qui reconnaît les images similaires.

Dans le cas de la comparaison d'image, nous pouvons nous inspirer des travaux de Joorabchi _et al._ [@joorabchi2012reverse] qui
    l'ont utilisé pour la génération des états pour leur méta-modèle de state flow.
