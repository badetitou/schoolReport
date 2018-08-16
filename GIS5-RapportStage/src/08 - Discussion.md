# Discussion {#sec:discussion}

Bien que les résultats obtenus sont encourageants.
Notre travail n'a été évalué que sur l'application _bac à sable_.
Cette application doit contenir tous les types d'éléments que doivent utilisé les développeurs de Berger-Levrault,
    cependant il est possible que des déviances dans le code soient présentes dans les applications en production et absent de l'application _bac à sable_.

La \secref{interfaceXML} présente un biais que nous acceptons de ne pas traiter.
Puis la \secref{discussionValidation} décrit les difficultés d'évaluation du prototype que nous avons conçu.
Enfin, la \secref{discussionLayout} et la \secref{discussionComportement} présentent deux parties de l'interface non traitée que
    nous avons identifiée.

## Interfaces depuis un fichier XML {#sec:interfaceXML}

En GWT, il est possible de définir une interface graphique grâce à un fichier XML.
Pour cela, les développeurs définissent dans le fichier XML les tags qui correspondent aux widgets.
Un widget contenu dans un autre est ainsi représenté par une balise XML à l'intérieur d'une autre balise.

Dans le cadre de Berger-Levrault, seule l'application _bac à sable_ utilise cette technique pour définir des interfaces.
Nous avons donc décidé de ne pas traiter l'importation des widgets pour les business pages définit de cette manière.

## Outil de validation {#sec:discussionValidation}

Un des problèmes que nous avons actuellement est l'évaluation de la migration.
Il est facile de prendre l'application source et l'application cible et de regarder _manuellement_
    si la migration a bien été effectuée, mais sur des applications de la taille de celles de Berger-Levrault
    nous avons besoin d'un système automatique.

Nous avons effectué des recherches dans la littérature sur les approches à adopter pour évaluer une migration d'application.
Cependant, nous n'avons pas trouvé de solution applicable à notre cas d'étude.

Certains papiers proposent de calculer le nombre de widgets migré et de le comparer par rapport au nombre réel de widgets dans l'application.
Cependant, dans notre cas le nombre de widgets total est trop grand.

## Gestion des layout {#sec:discussionLayout}

![KDM - Diagramme de classe UIRelations](figures/kdmRelations.png){#fig:uiRelations width=70%}

Comme expliqué \secref{respectContraintes}, certains résultats de l'exportation ne sont pas corrects à cause d'un manque de respect des layouts.
C'est le cas de la \figref{cmp2}.
Ceci est dû à la manière dont est retranscrite l'idée de layout dans le méta-modèle d'interface graphique.
Pour le moment, le layout est considéré comme un attribut, ce qui rend difficile la représentation d'interfaces complexes.
Plusieurs solutions peuvent être envisagées pour représenter les interfaces graphiques.

Gotti et Mbarki [@gotti2016java] ont décidé d'utiliser le méta-modèle proposé par KDM.
Le méta-modèle KDM, représenté \figref{uiRelations}, utilise une association entre deux UIRessource, via une entité UILayout,
    pour représenter la position de l'un part rapport à l'autre.
C'est ainsi qu'est défini le layout d'une application.
Le méta-modèle permet aussi de définir un _flow_ entre deux AbstractUIElement.

Sánchez Ramón _et al._ [@sanchez2014model] ont créé un méta-modèle pour les layouts.
Ce méta-modèle propose de lier la notion de widget avec celle de layout et de combiner les layouts afin de
    créer un layout précis.
Les auteurs ont défini un sous-ensemble de layout qu'il est possible de connecter aux widgets.
Cette solution est celle qui se rapproche le plus des techniques de conception que nous utilisons.

## Gestion du comportement {#sec:discussionComportement}

Pour le moment, nous n'avons représenté que les liens de navigation.
Afin d'améliorer la représentation que nous avons d'une application graphique,
    il faudrait que l'on représente toutes les couches d'une application comme décrite \secref{guiDecomposition}.
Nous devons encore définir et implémenter des méta-modèles pour le code comportemental et le code métier.
