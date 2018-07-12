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

## Processus de migration {#sec:processusMigration}

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
L'outil a été implémenté en Pharo[^pharo] et nous avons utilisé la plateforme Moose[^moose].
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
[^pharo]: [Pharo est un langage de programmation objet, réflexif et dynamiquement typé - (http://pharo.org/)](http://pharo.org/)

### Meta-modèle

Pour implémenter le méta-modèle GUI (voir Figure \ref{guiModel}), nous avons utilisé la dernière version de Famix dans Moose.
Cet outil est pratique car il fournit un générateur de méta-modèle, le builder.
Pour définir les entités du méta-modèles, il faut les nommer dans la méthode `defineClasses` du builder.
Pour les relations entre les entités, nous pouvons utilisé le format UML.
Ainsi une relation _"oneToMany"_ entre deux entités se définit de la manière suivante : `entity1 -* entity2`.

Afin de tester la stratégie sur l'application de Berger-Levrault,
    nous avons créé des types spécifiques de Widget pour Berger-Levrault.
Des exemple de ces widgets sont le _SplitButton_, _RichTextArea_ ou _Switch_.
Cette éléments n'appartiennent pas au modèle GUI d'origine mais,
    combiné avec le cadre que nous avons créé, ils rendent l'implémentation de l'outil plus modulaire.

### Importation

La création des modèles représentant l'interface graphique est divisée en trois étapes comme présenté Section \ref{sec:processusMigration}.
Dans le cas de Berger-Levrault, nous avons implémenté la stratégie en Pharo avec Moose.

La première étape est la conception du modèle de la technologie source.
Ce modèle avait déjà une implémentation existante dans Moose avec le projet _Famix-Java_.
Nous avons donc réutiliser ce modèle pour ne pas avoir à re-concevoir un modèle pré-existant.
De plus, ce travail préliminaire est compatible avec plusieurs outils qui ont été développé
    en interne à RMod.
Entre autre, deux logiciels de génération du model Famix-Java depuis du code source java existait.
Les outils sont verveineJ[^verveineJ] et jdt2Famix[^jdt2famix].
Ces deux derniers permettent de créer depuis le code source un fichier _mse_.
Le fichier _mse_ peut ensuite être importé dans la plateforme Moose.
Pour le cas de Berger-Levrault, nous avons utilisé verveineJ car ce dernier permet aussi de _garder un lien_ entre le modèle
    généré et le code à partir duquel il l'a été.

Une fois le modèle de la technologie source créé, et après avoir implémenté nos méta-modèles,
    nous avons développé des outils en Pharo permettant d'effectuer la transformation du modèle source vers le modèle GUI.
Nous allons maintenant décrire les techniques utilisées pour retrouver les éléments définit dans le modèle GUI depuis le modèle de technology source.

Les premiers éléments que nous avons voulu reconnaître sont les phases.
En analysant les projets GWT, nous avons repéré un fichier _.xml_ dans lequel est stocké toutes les informations des phases.
Nous avons donc ajouté une étape à l'importation qui est l'analyse d'un fichier _xml_.
Ce fichier nous permet de _"facilement"_ récupéré la classe java correspondant à une phase,
    ainsi que le nom de la phase.

Ensuite, nous avons développé l'outil d'importation de manière incrémentale.
Nous avons donc cherché les Business Page.
Grâce à l'analyse préliminaire des applications de Berger-Levrault, nous avons détecté que
    les business pages en GWT correspondent à des classes qui implémente l'interface _IPageMetier_.
Une fois les classes trouvées, nous avons recherché les appels des constructeurs des classes.
Puis, en faisant le lien entre le constructeur et la phase qui _"ajoute"_ la business page à leur contenu,
    nous avons détecté les pages métiers qui appartenu par chaque phase.

Pour les widgets, nous avons dû tout d'abord trouver tous les widgets potentiellement instanciable.
Pour cela, nous avons cherché toutes les sous-classes java de la classe GWT _Widget_.
Ce sont les classes qui vont pouvoir être instancié et utilisé pour la construction du programme.
Ensuite, comme pour les business pages, nous avons cherché les appeles des constructeurs des widgets et
    avons relié ces appeles à la business page qui les a ajouté.

Enfin, pour la détection des attributs et des actions associés à un widget.
Nous avons, pour chaque widget, cherché dans quelle variable java il a été affecté.
Puis nous avons cherché les appeles de méthodes effectué depuis ces variables java.
Les appeles aux méthodes _"addActionHandler"_ sont transformés en action tandis que
    les appeles aux méthodes _"setX"_ ont été transformé en attribut.

[^verveineJ]: [verveineJ : https://rmod.inria.fr/web/software/](https://rmod.inria.fr/web/software/)
[^jdt2famix]: [jdt2famix : https://github.com/feenkcom/jdt2famix](https://github.com/feenkcom/jdt2famix)

### Exportation

Une fois la génération du modèle d'interface graphique et du modèle du code comportemental terminé,
    il est possible de lancer l'exportation.
L'exportation consiste en la génération du code source de l'application cible.

La première étape de l'implémentation de l'exportation est l'utilisation
    d'un patron de conception _"visiteur"_.
Ce dernier est ajouté au modèle d'interface graphique, aux phases et business pages.

La visite du modèle GUI va créer la hiérarchie de l'application cible ainsi que les fichiers
    de configuration.
Ensuite, l'exportation visite toutes les phases.
Pour chacune des phases, considéré comme des sous-projet en Angular dans l'architecture de l'application cible que nous avons défini,
    le visiteur génère les fichiers de configurations.
Puis, pour chaque business page, le visiteur va générer un fichier HTML et un fichier TypeScript.
Pour le fichier html, le visiteur construit le DOM à partir des widgets contenu dans la business page.
Chacun des widgets connaissant ses attributs et actions,
    ils fournissent eux-même leurs caractéristiques aux visiteurs.
Ces caractéristiques englobent la génération du code comportemental.

\newpage