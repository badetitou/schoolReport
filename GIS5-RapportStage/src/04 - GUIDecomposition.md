# GUI Décomposition

La première étape pour créer les méta-modèles d'une application graphique
    est de définir les éléments que nous devons représenter.
Nous avons divisé l'interface en trois parties:

- L'interface utilisateur
- Le code de l'entreprise
- Le code de comportement

## Interface utilisateur

L'interface utilisateur est la partie visible.
Cet élément représente l'interface de l'application.
Il comprend les composants de l'interface.
L'interface utilisateur ne contient pas la visualisation exacte d'un composant.
Mais il peut préciser certaines caractéristiques inhérentes au composant, comme la possibilité d'être cliqué,
    ou certaines propriétés du composant, comme sa couleur ou sa taille.
Plus que les composants, il comprend également la disposition
    de ces composants par rapport aux autres.
Dans le cas où une application est composée de plusieurs fenêtres (ou de pages web pour une application web),
    l'interface utilisateur contient toutes les fenêtres.

## Code comportemental

Le code comportemental est la partie _executable_ de l'application.
Cela correspond à la logique de l'application.
Il peut avoir deux manifestations du code comportemental.
Il peut être exécuté soit par une action de l'utilisateur sur un composant d'interface (comme un clic) ou par le système lui-même.
Comme un langage de programmation _"classique"_, le code métier contient des structures de contrôle (_i.e._ boucle et alternative).
Lié à l'interface utilisateur, le code comportemental définit la logique de l'interface utilisateur.
Cependant, le code comportemental n'exprime pas la logique de l'application.
Cette partie est dédiée au code comportemental.

## Code métier

Le code métier définit les informations spécifiques d'une application.
Il est composé par les règles générales de l'application
    (comment calculer les taxes ?), le lien services distants (quel serveur mon code métier doit demander), les données de l'application (quelle base de données ? quel type de _object_).
Le code métier n'est donc pas directement lié à l'interface utilisateur.

\newpage