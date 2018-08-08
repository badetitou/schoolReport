# GUI Décomposition {#guiDecomposition}

Les applications que nous devons migrer ont des interfaces graphiques.
Avant de créer l'outil de migration, il faut comprendre ce qu'est une telle interface
    et comment nous pouvons la diviser.
Diviser un problème en petits sous-problèmes est une méthode efficace pour résoudre des problèmes complexes.
Nous avons identifié trois parties dans une interface graphique :

* L'interface utilisateur
* Le code de l'entreprise
* Le code comportemental

## Interface utilisateur

L'interface utilisateur est la partie visible.
Cet élément représente l'interface de l'application.
Elle comprend les composants de l'interface.
L'interface utilisateur ne contient pas la visualisation exacte d'un composant,
    mais elle peut préciser certaines caractéristiques inhérentes au composant, comme la possibilité d'être cliqué,
    ou certaines propriétés du composant, comme sa couleur ou sa taille.
Plus que les composants, elle décrit également la disposition
    de ces composants par rapport aux autres.
Dans le cas où une application est composée de plusieurs fenêtres (ou de pages web pour une application web),
    l'interface utilisateur contient toutes les fenêtres.

## Code comportemental

Le code comportemental est la partie _exécutable_ de l'application.
Cela correspond à la logique de l'application.
Il peut avoir deux manifestations du code comportemental.
Il peut être exécuté soit par une action de l'utilisateur sur un composant d'interface (comme un clic) ou par le système lui-même.
Comme un langage de programmation _"classique"_, le code métier contient des structures de contrôle (_c.-à-d._ boucle et alternative).
Lié à l'interface utilisateur, le code comportemental définit la logique de l'interface utilisateur.
Cependant, le code comportemental n'exprime pas la logique de l'application.
Cette partie est dédiée au code comportemental.

## Code métier

Le code métier définit les informations spécifiques d'une application.
Il est composé par les règles générales de l'application
    (comment calculer les taxes ?), l'adresse des services distants (quel serveur mon code métier doit demander), les données de l'application (quelle base de données ? quel type d'objet).
Le code métier n'est donc pas directement lié à l'interface utilisateur.

\newpage