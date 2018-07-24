# Travaux futurs

Nous avons réussi à construire des bases solides pour améliorer l'outil de migration que nous avons commencer.
Afin de l'améliorer, il faudrait encore ajouter la gestion des layout, du code comportemental et métier.
Nous allons devoir corriger les quelques erreurs restante qui nous empêche d'importer 100%
    des widgets que nous souhaitons exporter.
Enfin, nous devons définir une ou plusieurs stratégies pour évaluer correctement la complétude de notre travail.

## Amélioration des modèles

Comme expliqué Section \ref{exportToAngular}, certains résultat de l'exportation ne sont pas correct à cause d'un manque de respect des layouts.
C'est le cas de la Figure \ref{cmp2}.
Ceci est dû à la manière dont est retranscrit l'idée de layout dans le méta-modèle d'interface graphique.
Pour le moment, le layout est considéré comme un Attribut, il est donc difficile de représenter des comportement complexe.
Plusieurs solution peuvent être envisagé pour représenter les interfaces graphiques.

Gotti et Mbarki [@gotti2016java] ont décidé d'utiliser le meta-modèle proposé par KDM [^kdm].
Le méta-modèle KDM de layout utilise une association entre deux _AbstractUIElement_ pour représenter la position de l'un part rapport à l'autre.
C'est ainsi qu'est défini le layout d'une application.

Sánchez Ramón _et al._ [@sanchez2014model] ont crée un méta-modèle pour les layouts.
Ce méta-modèle propose de lier la notion de widget avec celle de layout et de combiner les layouts afin de
    créer un layout précis.
Les auteurs ont défini un sous-ensemble de layout possible a connecter aux widgets.
Cette solution est celle qui se rapproche le plus des technique de conception que nous utilisons.

[^kdm]: KDM : Knowledge Discovery Metamodel

## Outil de validation

## Complétude du travail

## Exportation du Core

- behavior complet
- business code
- outil de validation
- missing widget

\newpage