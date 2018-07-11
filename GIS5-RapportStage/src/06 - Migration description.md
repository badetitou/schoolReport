# Mise en place de la migration par via les modèles

Suite à l'étude des contraintes inhérent au problème de migration dans le cadre
    d'une entreprise.
Et après la recherche de l'état de l'art.
Nous avons travaillé sur la conception et l'implémentation d'une stratégie de migration
    respectant les critères que nous avons fixés.

Comme vu Section \ref{sec:strategieMigration}, seul la migration en utilisant les modèles nous permet
    de respecter toutes les contraintes.
Ce type de migration nous impose la conception de méta-modèles.

Nous allons présenter dans cette partie le processus de migration que nous avons conçu,
    puis le méta-modèle d'interface utilisateur utilisé dans le ce processus,
    et enfin expliquer l'implémentation de cette stratégie.

## Processus de migration

![Schema processus de migration](figures/processusMigration.png){#fig:processusMigration width=100%}

A partir de l'état de l'art et des contraintes que nous avons explicités,
    nous avons conçu une stratégie pour effectuer la migration.
Le processus que l'on a représenté Figure \ref{fig:processusMigration} est divisé en cinq étapes :

1. _Extraction du modèle de la technologie source_ est la première étape permettant de construire l'ensemble des analyses et transformations que nous devons appliquer pour effectuer la migration. Elle consiste en la génération d'un modèle représentant le code source de l'application originel. Dans notre cas d'étude, le programme source est en java et donc le modèle que nous créons est une implémentation d'un méta-modèle permettant de représenter une application écrite en java.
2. L'_Extraction de l'interface utilisateur_ est l'analyse du modèle de la technologie source pour détecter les éléments qui relève du modèle d'interface utilisateur. Ce dernier, que nous avons dû concevoir, est expliqué Section \ref{sec:metamodelUI}.
3. _Extraction du code comportemental_. Une fois le modèle d'UI généré, il est possible d'extraire le code comportemental du modèle de la technologie source et de créer les correspondances entre les éléments faisant partie à la fois du code comportemental et du model d'interface utilisateur. Par exemple, si un clique sur un bouton agit sur un texte dans l'interface graphique. L'extraction du code comportemental permet de définir que pour le bouton, définit dans le model UI, lorsqu'un clique est effectué on effectue un certain nombre d'action dont une sur le texte, lui aussi définit dans le modèle UI.
4. _Exportation de l'interface utilisateur_. Le modèle d'interface graphique étant construit et les liens entre interface utilisateur et code comportemental créés, il est possible d'effectuer l'exportation de l'interface utilisateur. Cela consiste à la génération du code du langage source exprimant uniquement l'interface graphique. C'est aussi à cette étape que l'on génère l'architecture du des fichiers nécessaire au fonctionnement de l'application cible ainsi que la création des fichiers de configuration inherent à l'interface.
5. Finalement, l'_Exportation du code comportemental_ est la génération du code comportemental qui est lié à l'interface utilisateur. Cette étape peut être effectuée en parallèle de la quatrième.

## Méta-modèle d'interface utilisateur {#sec:metamodelUI}

![Méta-Modèle d’un application de Berger-Levrault](figures/guiModel.png){#guiModel width=80%}

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

Pour tester la stratégie, nous avons implémenté un outil qui suit le processus de migration.
L'outil a été implémenté au Pharo et nous avons utilisé la plateforme Moose[^moose].
Moose est une plateforme pour l'analyse de logiciels et de données.

![Implémentation de l'outil](figures/codeImpl.png){#codeImpl width=350px height=250px}

Le Figure \ref{codeImpl} présente la logique d'implémentation.
Le bloc principal est _BL-Model_.
Ce bloc contient l'implémentation du méta-modèle GUI.
En plus du modèle, il y a un exportateur abstrait et une implémentation de
    l'exportateur pour Angular (_BL-Model-Exporter_ et _BL-Model-Exporter-Angular_, un importateur abstrait et le    code spécifique pour Java (_BL-Model-Importateur_ et _BL-Model-Importer-Java_).
Parce que nous testons notre solution sur le système de Berger-Levrault,
    nous avons également implémenté l'extension _"CoreWeb"_,
    alors que la stratégie de migration ne dépend pas de cette extension.
Ces paquets étendent les précédents pour avoir un contrôle fin du processus de migration.
Ce contrôle est important pour améliorer le résultat final.

[^moose]: [http://www.moosetechnology.org/](http://www.moosetechnology.org/)

### Meta-modèle

Pour implémenter le méta-modèle GUI (voir Figure \ref{guiModel}), nous avons utilisé la dernière version de Famix dans Moose.
Cet outil est pratique car il fournit un générateur de méta-modèle.
Définir les entités et leurs relations est simple.
Afin de tester la stratégie sur l'application de Berger-Levrault,
    nous avons créé un type spécifique de Widget pour Berger-Levrault.
Il y a le widget spécifique utilisé par Berger-Levrault tel que _SplitButton_, _RichTextArea_, _Switch_, _etc._
Cette partie n'appartient pas au modèle GUI d'origine mais,
    combiné avec le cadre avec créé, il rend la chose plus facile à étendre et à personnaliser.

### Importation

### Exportation

\newpage