# Décomposition d'une application avec interface graphique {#sec:guiDecomposition}

Avant de créer l'outil de migration,
    nous avons étudié puis décomposé la notion d'interface graphique.
Diviser un problème en sous-problèmes est une méthode efficace pour résoudre des problèmes complexes.
Nous avons identifiées trois parties dans une interface graphique :

* La partie visuelle
* Le code comportemental
* Le code métier

## Partie visuelle

La partie visuelle peut être mise en correspondance avec le méta-modèle UI de KDM (voir \secref{omg}).
Elle contient les différents éléments de l'interface.
La partie visuelle définit les caractéristiques inhérentes aux composants, comme la possibilité d'être cliqué,
    ou certaines propriétés du composant, comme sa couleur ou sa taille.
Plus que les composants, elle décrit également la disposition
    de ces composants par rapport aux autres.
Nous avons vu que cela est souvent représenté grâce au patron de conception  _composite_.
Dans le cas où une application est composée de plusieurs fenêtres (ou de pages web pour une application web),
    la partie visuelle contient toutes les fenêtres.

## Code comportemental

Le code comportemental défini le flot d'action qui s'exécute lorsqu'un
    utilisateur interagit avec la partie visuelle de l'application.
L'utilisateur peut effectuer une action sur un composant de l'interface (comme un clic).
Il est aussi possible que ce soit le système lui-même qui décide de déclencher une suite d'action suite à un événement extérieur.
Comme dans un langage de programmation _"classique"_, le code comportemental contient des structures de contrôle (_c.-à-d._ boucle et alternative).

## Code métier

Le code métier inclus tout ce qui n'est définit ni dans la partie visuelle ni dans le code comportemental.
Cela correspond à tout ce qui est lié à l'application, mais pas à la partie visuelle.
Il est composé des règles générales de l'application
    (comment calculer les taxes ?), de l'adresse des services distants (quel serveur mon code métier doit demander), des données de l'application (quelle base de données ? quel type d'objet).
