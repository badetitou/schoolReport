# Travaux futurs {#sec:futurWork}

Nous avons réussi à construire des bases solides pour l'outil de migration.
Afin de l'améliorer, il faudrait encore ajouter la gestion du code comportemental comme présenté \secref{futurComportementale}.
Nous allons aussi travailler sur la conception d'une approche pour évaluer
    les résultats que produit notre outil.

## Code comportementale {#sec:futurComportementale}

Afin de représenter le comportement des widgets de l'interface graphique,
    nous avons mené une étude préliminaire de la littérature sur la représentation du code comportemental \secref{metamodelComportemental}.
Ensuite, présenté \secref{metamodelComportemental}, nous avons commencé la conception d'un méta-modèle pour représenter le code comportementale.

### Etude de l'art pour le code comportementale {#sec:stateMetamodelComportemental}

On retrouve dans la littérature deux méta-modèles très présents.
Le méta-modèle de navigation et le méta-modèle de _state flow_.

Le méta-modèle de navigation permet de représenter un lien entre deux pages web ou fenêtres différentes.
Le lien peut être fait d'une page web à une autre, ou d'un widget vers une page web.
Le méta-modèle de navigation peut aussi contenir un lien d'un widget vers un événement, et un autre de cet événement vers
    une page web.

Morgado _et al._ [@morgado2011reverse] et Fleurey _et al._ [@fleurey2007model] ont utilisé un méta-modèle de navigation
    pour représenter les liens entre les différentes interfaces utilisateur qu'ils détectent.
Les premiers utilisent un méta-modèle supplémentaire qui décrit simplement l'ensemble des _fenêtres_ possible dans l'application.
Son méta-modèle de navigation permet de faire le lien entre une action sur un widget et l'action de navigation qui en résulte.

Pour Fleurey _et al._, le méta-modèle contient directement un lien entre la fenêtre de départ et celle d'arrivée.
Le méta-modèle possède tout de même une entité appelée _Operation_, mais qui ne semble pas impliquée dans la navigation.

Les informations de ces méta-modèles sont complètement contenues dans notre méta-modèle du code comportemental.
Donc, bien que nous n'ayons pas de méta-modèle de navigation, nous faisons au moins ce qui est proposé dans ces papiers.

Le méta-modèle de _state flow_ permet de créer le lien entre différents états de l'interface utilisateur.
Un état de l'interface utilisateur est défini par les widgets visibles et leurs propriétés.
Ainsi, si la valeur d'une propriété change, un nouvel état est généré.

Les auteurs Memon _et al._ [@memon2007eventflow], Mesbah _et al._ [@mesbah2012crawling], Amalfitano _et al._ [@amalfitano2012using]
    , Silva _et al._ [@silva2010guisurfer] et Aho _et al._ [@aho2013industrial] ont tous utilisé un méta-modèle de _state flow_
    afin de représenter les différentes transitions entre les interfaces graphiques.
Pour définir un état, leurs outils vont analyser les valeurs des propriétés des widgets visibles sur un écran.
Une fois une action exécutée, l'outil détecte si l'état d'un widget a changé, dans ce cas un nouvel état de l'application est créé.
Ainsi, les auteurs sont capables de représenter les impacts d'une action sur l'interface graphique.

Pour définir un état, Joorabchi _et al._ [@joorabchi2012reverse] ont décidé d'effectuer une comparaison d'image.
Après chaque action sur un widget, exécuté par un outil, ils prennent une image de l'application et la comparent avec une image prise avant l'action.
Si les deux images sont différentes, alors les auteurs ont découvert un changement d'état provenant d'une action.

Le méta-modèle de _state flow_ permet de représenter l'impact d'un événement sur l'interface utilisateur.
Le code à executer sur l'interface afin de passer d'un état à un autre est contenu dans notre méta-modèle du code comportemental.

Notre méta-modèle de code comportemental permet de représenter les informations
    contenues par les méta-modèle de navigation et de _state flow_.
Cependant, nous ne représentons pas l'état d'entrée et l'état de sortie après une action,
    mais l'état d'entrée d'une fenêtre et la logique à appliquer après une action pour obtenir l'état de sortie.
Cette différence est dû à notre objectif qui est de migrer cette logique dans un nouveau langage et non
    d'analyser les différents états possibles de l'application.

### Méta-modèle du code comportemental {#sec:metamodelComportemental}

![Méta-Modèle du code comportemental](figures/behavioralModel.png){#fig:behavioralModel width=100%}

Le méta-modèle du code comportemental présenté \figref{behavioralModel} est un prototype de méta-modèle sur
    lequel nous travaillons pour représenter le code lié au comportement de l'application.
Il y a deux éléments principaux, Statement et Expression.

Le **Statement** est la représentation des structures de contrôle des langages de programmation.
Il y a l'alternative, la boucle, la notion de bloc et un lien vers une expression.

Une **Expression** représente un morceau de code qui peut être évalué directement.
Cela peut être une affectation avec la valeur de l'affectation,
    un littéral (directement la valeur de l'élément écrit),
    une expression booléenne,
    une expression arithmétique
    ou une invocation.
Dans le cas d'une invocation, c'est la valeur de retour de la méthode
    qui est utilisée comme valeur de l'expression.

Les éléments **Action** constituent le lien vers le modèle d'interface graphique.
C'est le conteneur de la logique d'un événement déclenché par une action.

Grâce à ce modèle, nous pouvons représenter la logique
    exécutée par un lorsqu'un événement est déclenché par une action sur un widget
    du modèle d'interface graphique.

## Evaluation de la migration {#sec:validationFutur}

Nous n'avons pas trouvé de stratégie ou de métriques pour évaluer précisément la validité de
    notre prototype.
Il est donc important que l'on recherche des stratégie pour apprécier la qualité de notre approche.

L'évaluation devra nous permettre de vérifier l'aspect visuelle, le code comportemental et le code métier
    de l'application à migrer ont bien été migré.

Pour l'aspect visuelle, il est possible de comparer les DOM des applications car nous travaillons avec des applications web.
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

Dans le cas de la comparaison d'image, nous pouvons nous inspirer des travaux de Joorabchi _et al._ [@joorabchi2012reverse] qui
    l'ont utilisé pour la génération des états pour leur méta-modèle de state flow.
