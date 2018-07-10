# Mise en place de la migration par via les modèles

## Processus de migration

## Meta-modèle d'interface utilisateur

![Méta-Modèle d’un application de Berger-Levrault](figures/guiModel.png){#guiModel width=350px height=250px}

Afin de représenter une interface utilisateur, nous avons conçus le méta-model proposé Figure \ref{guiModel}.
Dans la suite de cette partie, nous présentons les différentes entités du méta-modèle.

La __Phase__ représente le conteneur principal d'une page interface utilisateur.
Cela peut correspondre à une _fenêtre_ d'une application du bureau, une page web, ou
    dans notre cas d'un onglet d'une page web.
Une Phase peut contenir plusieurs Business page.
Elle peut aussi être appelé par un widget grâce à une Call Phase Action.
Lorsqu'une Phase est appelée, l'interface change pour afficher la Phase.
Dans le cas d'une application de bureau, l'interface change ou une nouvelle fenêtre est ouverte
    avec l'interface de la Phase.
Avec une application web, l'appelle d'une Phase peut correspondre à l'ouverture d'un nouvel onglet, le changement d'onglet actif ou le transformation de la page web courante.

Les __Widgets__ sont les différents composants d'interface et les composants de disposition.
Il existe deux types de widgets.
Le __Leaf__ est un widget qui ne contient pas un autre widget dans l'interface.
Le __Containers__ peut contenir un autre widget.
Ce dernier permet de séparer les widgets en fonction de leur place dans l'organisation de la page web représenté.

Le __Attribute__ représente les informations appartenant à un Widget et peut changer son aspect visuel ou son comportement.
Les attributs communs sont la hauteur et la largeur pour définir précisément la dimension d'un widget.
Il y a aussi des attributs pour contenir des attributs tels que le texte contenu par un widget.
Par exemple, un widget représentant un bouton peut avoir un attribut _text_ explicite le texte du bouton.
Un attribut peut changer le comportement, ce pourrait être le cas d'un attribut _enable_.
Un bouton avec l'attribut _enable_ positionné sur _false_ représente un bouton nous ne pouvons pas cliquer.
Enfin, les widgets peuvent avoir un attribut qui aura un impact sur le visuel de l'application.
Cet attribut définit la disposition de ses enfants et potentiellement sa propre dimension pour respecter la mise en page.

Les __Actions__ sont propres aux Widgets.
Ce sont des actions qui peuvent être exécutées dans une interface graphique.
__Call Service__ représente un appel à un service distant tel Internet.
__Fire PopUp__ est l'action qui affiche un PopUp sur l'écran.
Le PopUp ne peut pas être considéré comme un widget,
    il n'est pas présent dans l'interface graphique,
    il apparaît seulement et disparaît.

Le __Service__ est la référence à la fonctionnalité distante que l'application peut appeler à partir de son interface graphique.
Dans un contexte Web, il peut s'agir du côté serveur de l'application.

## Implémentation du processus

![Implémentation de l'outil](figures/codeImpl.png){#codeImpl width=350px height=250px}

### Meta-modèle

### Importation

### Exportation

\newpage