# Discussion {#sec:discussion}

Bien que nous ayons testé régulièrement notre travail sur les applications de production de Berger-Levrault,
    la recherche des patterns pour l'importation ainsi que l'évaluation de la migration n'a été faîte que sur l'application
    _bac à sable_.
Nous savons que l'application après migration compile, mais nous n'avons pas de retours sur la réussite de l'exportation du visuel.
Il est aussi possible que les autres logiciels de Berger-Levrault contiennent des déviances dans le code que nous n'avons pas prévu
    ce qui peut nuire au résultat final.

En GWT, il est possible de définir une interface graphique grâce à un fichier XML.
Dans le cadre de ce projet, seule l'application _bac à sable_ utilise cette technique pour définir des interfaces.
Nous avons donc décidé de ne pas traiter l'importation des widgets pour les business pages définit de cette manière.
En les considérant, le pourcentage de widget que nous arrivons à importer se voit réduit.

## Gestion des layout {#sec:discussionLayout}

Comme expliqué \secref{exportToAngular}, certains résultats de l'exportation ne sont pas correct à cause d'un manque de respect des layouts.
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

## Gestion du comportement {#sec:discussionComportement}

Afin d'améliorer la représentation que nous avons d'une application graphique,
    il faudrait que l'on représente toutes les couches d'une interface graphique comme décrite \secref{guiDecomposition}.
Une fois le layout implémenté nous pourrons bien représenter l'interface utilisateur, il nous faudra encore définir et implémenter
    des méta-modèles pour le code comportemental et le code métier.

Nous avons déjà défini un méta-modèle de code comportemental qui est présenté \secref{metamodelComportemental}.
Il faut encore implémenter ce modèle et modifier en conséquence les importateur et exportateur que nous avons créés.
Cette amélioration devrait nous permettre de mieux représenter les comportements de l'application
    lorsqu'un utilisateur effectue une action sur l'interface.

Enfin, il nous faut concevoir un méta-modèle pour le code métier.
Cela devrait permettre d'extraire et représenter les règles métiers d'une application.
