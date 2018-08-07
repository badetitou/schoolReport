# Etat de l'art

Dans le cadre de la conception de l'outil de migration,
    nous avons étudié la littérature pour identifier
    les techniques respectant les critères définit par Berger-Levrault.
Dans un premier temps, la Section \ref{migrationTechnique} présente les différentes
    techniques qui sont utilisés pour faire effectuer la migration d'application.
Dans un second temps, la Section \ref{positionnement} positionnera notre travail
    par rapport à ceux proposé dans la littérature et utilisant la même technique
    de migration.

## Technique de migration {#migrationTechnique}

Nous avons extrait de la littérature plusieurs domaine de recherche connexe au
    travail que nous voulons mener sur la migration d'application.
Les approchent proposées permettent soit d'effectuer la migration d'application
    sans répondre aux critères que nous avons définit avec Berger-Levrault, soit
    de proposer des pistes à explorer pour créer une solution.

### Rétro-Ingénierie d'interface graphique

La rétro-ingénierie discute de comment représenter une interface graphique
    dans l'objectif de pouvoir l'analyser et comment générer cette représentation.
Le choix de représentation des interfaces graphiques est discuté Section \ref{positionnement}.

Les auteurs utilisent trois techniques pour instancier leurs méta-modèles.

**Statique**. La stratégie statique consiste à analyser le code source de l'application comme on analyserait du texte. en fonction du language source, l'analyse statique peut être plus ou moins facile à mettre en place.

Dans le cas de [@cloutier2016wavi], les auteurs ont pû analyser directement les fichier HTML, CSS et javascript.
Ceci leurs permet de construire un arbre syntaxique du code source du site web
    et d'extraire les différents widgets depuis le fichier html.

D'autre auteurs comme [@silva2010guisurfer;@lelli2016automatic;@staiger2007reverse] utilisent un outil qui va analyser du code source provenant de langage non destiné au web mais permettant de décrire une interface graphique.
Leurs cas d'études sont des applications de bureau ayant une interface graphique.
Ces logiciels vont chercher la définissions des widgets dans le code source.
Une fois les créations des widgets trouvés dans le code source, le logiciel va analyser méthodes invoquées ou invoquant les widgets afin de découvrir les relations entre les widgets et leurs attributs.

Sánchez Ramón *et al.* [@sanchez2014model] ont développé une solution permettant d'extraire depuis un ancien logiciel son interface graphique.
Le cas d'étude des auteurs est une application source ayant était créée avec Oracle Forms.
Contrairement aux cas d'études précédant, l'interface est définit dans un fichier à part explicitant pour chaque widget sa position fixe.
Chaque widget a ainsi une position X, Y ainsi qu'une hauteur et une largeur.
La stratégie de l'auteur consiste à determiner la hiérarchie des widgets depuis ces informations.

**Dynamique**. La stratégie statique consiste à analyser l'interface graphique
    d'une application pendant qu'elle est en fonctionnement.
L'utilisateur va lancer un logiciel dont l'interface est à extraire,
    puis il va lancer l'outil d'analyse dynamique.
Ce dernier va être capable de détecter les différents composants de l'interface et les actions qu'il peut leurs appliquer.
Puis l'outil va appliquer une action sur un élément de l'interface est détecter les changements s'il y en a.
En répétant ces actions, la stratégie va permettre d'explorer les différentes interfaces qui sont utilisées dans l'application à analyser et comment interagir avec ces dernières.

Les auteurs [@MemonWCRE2003;@samir2007swing2script;@shah2011reverse;@morgado2011reverse] ont développé des logiciels implémentant la stratégie dynamique.
Cependant, toutes les solutions proposées s'applique sur des applications exécutées sur un ordinateur (application de bureau).
Une adaptation serait nécessaire pour coller au spécificité des applications web comme dans notre cas.

**Mixe**. L'objectif de la stratégie mixe est d'utiliser la stratégie statique et la stratégie dynamique.
Gotti _et al._ [@gotti2016java] utilise une stratégie mixe pour l'analyse d'applications écrites en Java.
La première étape consiste en la création d'un modèle grâce à une analyse statique du code source.
Les auteurs retrouvent la composition des interfaces graphiques, les différents widgets et leurs propriétés.
Ensuite, l'analyse dynamique va executer les différentes actions possibles sur tout les widgets et
    analyser les modifications potentielles sur l'interface.

### Transformation de modèle vers modèle

La _transformation de modèle vers modèle_ traite de la modification d'un modèle source vers un modèle cible.

L'article de Baki *et al.* [@baki2016multi] présente un processus de migration d'un modèle UML vers un modèle SQL.
Pour faire la migration, les auteurs ont décidé d'utiliser des règles de transformation.
Ces règles prennent en entrée le modèle UML et donne en sortie le SQL définit par les règles.
Plutôt que d'écrire les règles de migration à la main.
Les auteurs ont décomposé ces règles en petites briques.
Chaque brique peut correspondre soit à une condition à respecter pour que la règle soit validée, soit à un changement sur la sortie de la règle.
Ensuite, les auteurs ont développé un algorithme de programmation génétique pouvant manipuler ces règles.
A partir d'exemples l'algorithme apprend les règles de transformation à appliquer afin d'effectuer la transformation du modèle.
Pour cela, il modifie les petites briques composant les règles et analyser si le modèle en sortie ressemble à
  celui explicité pour tous les exemples.
Enfin, l'algorithme est appliqué sur de vrais données.
Ce travail peut être utilisé dans mon projet.
En effet, je peux aussi effectuer la migration en utilisant un modèle de l'application source et un
  modèle de l'application cible.

Wang *et al.* [@wang2017automatic] ont créé une méthodologie et un outil permettant d'automatiquement faire la transformation d'un modèle vers un autre modèle.
Leur outil se distingue en effectuant une migration qui se base sur une analyse syntaxique et sémantique.
L'objectif de la méthodologie est d'effectuer la transformation d'un modèle vers un autre de manière itérative en modifiant le méta-modèle.
Une condition d'utilisation contraignante décrite par les auteurs est la nécessité d'avoir
  un méta-méta-modèle pour tous les méta-modèles intermédiaires.
Les auteurs ont implémenté un méta-méta-modèle dans leur outil.
Dans mon travail, une solution pour effectuer la migration serait d'utiliser des méta-modèles.
Le premier modèle proviendrait de l'application source, le second modèle respecterait le méta-modèle de destination.
J'ai donc la même problématique que les auteurs de passage d'un modèle à un autre.
En définissant un méta-méta-modèle que respecterai le méta-modèle de départ de l'application source et un méta-modèle de destination,
  la méthodologie proposée par les auteurs devrait pouvoir résoudre, totalement ou partiellement, mon problème de migration.

![Migration selon le _fer à cheval_](figures/horseshoe.png){#horseshoe}

Fleurey *et al.* [@fleurey2007model] et Garcés *et al.* [@garces2017white] ont travaillé sur la modernisation et la migration de logiciel.
Ils ont développé des logiciels permettant de semi-automatiser la migration d'applications.
Pour cela, ils ont suivit le principe du _"fer à cheval"_ présenté Figure \ref{horseshoe}.
La migration se passe en quatre étapes.

1. Ils génèrent un modèle de l'application à migrer.
2. Ils transforment ce modèle en un "modèle pivot". Ce dernier contient la structure des données, les actions et algorithmes, l'interface graphique et la navigation dans l'application.
3. Ils transforment le modèle pivot en un modèle respectant le méta-modèle du langage cible.
4. Ils génèrent le code source final de l'application.

Comme les auteurs, je vais devoir faire la migration d'une application et je vais devoir conserver structure de données, actions, interface graphique et navigation dans l'application.
Mon travail peut donc s'inspirer de celui proposé par les auteurs si nous souhaitons utiliser les modèles pour effectuer la migration.

### Transformation de modèle vers texte

La _transformation de modèle vers texte_ traite du passage d'un modèle source vers du texte.
Le texte peut être du code source compilable ou non.

L'article de Mukherjee *et al.* [@mukherjee2011automatic] présente un outil permettant de prendre en entrée les spécifications d'un programme et donne en sortie un programme utilisable.
L'entrée est un fichier en XML et la sortie est un programme écrit en C ou en Java (en fonction du choix de l'utilisateur).
Pour effectuer les transformations, les auteurs ont utilisé un système de règles de transformation.
Je pourrai réutiliser ce travail si je passe par un modèle pour la migration.
En effet, le fichier XML pris en entrée de l'outil des développeurs peut être assimilé à un modèle suivant un méta-modèle (définit dans l'article).
Dans le cas où nous utilisons un modèle dans le cadre de la migration des applications de Berger-Levrault.
Nous pourrions aussi être amené à utiliser un système de règle pour faire la migration du modèle au code source.

### Migration de librairie

La _migration de librairie_ présente des solutions sur comment changer de framework.
Ce travail est lié à notre problématique puisque, pour la migration des applications de Berger-Levrault, nous passons de l'utilisation du framework GWT à l'utilisation de frameworks associé à Angular.

Chen *et al.* [@chen2016mining] ont développé un outil permettant de trouver des librairies similaires à une autre.
Pour cela, les auteurs ont miné les tags des questions de Stack Overflow.
Avec ces informations, ils ont pu mettre en relation des langages et leurs libraries ainsi que des équivalences entre librairies de langage différent.
Pour la migration de Java/GWT vers Angular, il est possible que nous ayons besoin de changer de librairie (qui n'est pas BLCore).
Plutôt que de récrire la librairie, la recherche d'une autre librairie permettant de résoudre les même problème peut être une solution.
C'est dans ce contexte que le travail des auteurs peut guider notre recherche de librairie en mettant faisant correspondre les anciennes librairies utilisées par les applications de Berger-Levrault avec d'autre compatible utilisable avec Angular.

### Migration de langage

La *migration de langage* traite de la transformation du code source directement (*i.e.* sans passer par un modèle).
Pour cela, les auteurs créent des "règles" permettant de modifier le code source.

Brant *et al.* [@brant2010extreme] ont écrit un compilateur utilisant un outil nommé SmaCC.
SmaCC est un générateur d'analyseur pour Smalltalk.
Ils ont aussi utilisé le SmaCC Transformation Toolkit qui permet de définir des règles de transformations qui seront utilisées par SmaCC.
Ainsi, les auteurs sont parvenus à migrer une application Delphi de 1,5 million de lignes de code en C#.
Comme les auteurs, je veux effectuer la migration du code source d'une application.
Mon cas se différencie par les langages source et cible.
Ce travail peut me servir si nous souhaitons effectuer la migration sans passer par un modèle intermédiaire.

Un des problèmes de la migration du code source est la définition des règles.
Newman *et al.* [@newman2017simplifying] ont proposé un outil facilitant la création de règle de transformation.
Pour cela, l'outil "normalise" le code source en entré et essayer de le simplifier.
Ainsi, les auteurs arrivent à réduire le nombre de règles de transformations à écrire et leurs complexités.
Dans le cas de migration de Berger-Levrault, je dois gérer les multiples manières dont les fonctionnalités sont écrites.
La normalisation du code source peut simplifier l'écriture des règles de transformation ou
  les règles permettant de créer un modèle.

Rolim *et al.* [@rolim2017learning] ont créé un outil qui apprend des règles de transformation de programme à partir d'exemple.
Pour cela, les auteurs ont défini un DSL[^DSL] permettant d'exprimer les modifications faîtes sur l'AST[^ast] d'un programme.
Ensuite, à partir d'une base d'exemple de transformation, l'outil recherche les règles de transformation entre les fichiers d'entré des exemples
  et ceux de sorties.
Une fois les règles trouvées et écrites dans le DSL prédéfini, l'outil prend en entrée un bout de code et donne en sortie le résultat des transformations.
Ce travail peut nous servir pour la migration des applications de Berger-Levrault.
En effet, nous pouvons imaginer faire la migration de tout ou partie des applications en utilisant un tel outil.
De plus, on peut imaginer un outil qui apprendrai au fur et à mesure des développement des développeur et qui les
  conseillerez dans un second temps sur d'autre développement avec les même problématiques déjà résolu.
Ou encore, l'outil pourrait servir de "guide" pour les nouveaux développeurs participants à la migration ou
  au développement courant des applications.

[^ast]: AST : Arbre Syntaxique Abstrait
[^DSL]: DSL : Domain Specific Language est un langage de programmation destiné à générer des programme dans un domaine spécifique.

## Positionnement sur la migration via les modèles {#positionnement}

Comme décrit Section \ref{sec:processusMigration}, nous avons décidé d'effectuer la migration en utilisant les modèles.
Nous avons utilisé une stratégie selon le _fer à cheval_.

Afin d'appliquer cette stratégie de migration, nous avons dû définir plusieurs méta-modèles décrit Section \ref{sec:metamodelUI} et Section \ref{metamodelComportemental}.
Dans cette Section, nous allons nous positionner par rapport aux modèles proposées dans les autres papiers traitant de la représentation d'une application contenant une interface graphique.
Puis nous présenterons et discuterons les modèles KDM et IFML qui ont été proposées par l'OMG[^OMG] [(https://www.omg.org/spec/KDM/)](https://www.omg.org/spec/KDM/) pour
    représenter respectivement des applications et des interfaces graphiques.

[^OMG]: OMG : Object Management Group

### Méta-modèle d'interface utilisateur

### Méta-modèle de navigation et de state flow

### KDM

L'OMG a défini la norme KDM[^KDM] pour prendre en charge l'évolution des logiciels.
Le standard est un méta-modèle pour représenter un logiciel dans un haut niveau d'abstraction.

![Architecture KDM](figures/kdm.png){#fig:kdm width=80%}

Le Figure \ref{fig:kdm} présente un aperçu de l'architecture KDM.
Il est divisé en douze paquets organisés en quatre couches.
Le paquet d'interface utilisateur est composé d'un ensemble de méta-modèles à représenter
    les composants et le comportement de l'interface utilisateur.
L'action de package définit des méta-modèles pour représenter le comportement d'une application.
Il peut être utilisé pour décrire la logique de l'application, par exemple conditions, boucle, appel.
Il est similaire à notre méta-modèle de comportement.
Le package de données représente les données de différentes bases de données.
Il définit également un méta-modèle pour l'action de la base de données, par exemple, Insérer, Mettre à jour, Supprimer.
Ces actions peuvent être liées à un déclencheur et exécutées automatiquement lorsqu'un événement se produit.

![KDM Diagramme de Classe UIResources](figures/kdmUI.png){#fig:kdmUI width=80%}

La Figure \ref{fig:kdmUI} est le diagramme de classes UIResources.
Il définit de nombreuses entités pour l'abstraction d'une interface utilisateur.

UIResource peut être défini comme UIDisplay, UIField ou UIEvent.
Un UIField est utilisé pour représenter des formulaires, des champs de texte, un panneau, etc.
Il est similaire au widget d'entité de notre modèle d'interface utilisateur.
Nous pouvons également détecter le modèle composite avec
    UIResource et AbstractUIElement.
Ce motif fait la représentation du architecture d'interface utilisateur possible.

UIDisplay peut être un écran ou un rapport.
L'écran représente une fenêtre pour un logiciel de bureau ou une page Web.
Le rapport représente un élément qui sera imprimé.
Cette différence entre Report et Screen n'est pas représentée dans nos méta-modèles.

Chaque AbstractUIElement peut avoir une UIAction.
Une UIAction est utilisée pour représenter la séquence d'affichage dans une interface utilisateur.
Cela inclut la logique pour passer d'une interface utilisateur à une autre.
Ce n'est pas un modèle de navigation ni un modèle de flux d'état et c'est similaire à notre modèle de comportement.

Une UIAction peut effectuer plusieurs événements UIE.
UIEvent représente les événements appartenant à une interface utilisateur.
Il peut s'agir de la notion de rappel ou de l'idée de navigation (\ ie passer d'une page Web à une autre).

![KDM Diagramme de classe UIRelations](figures/kdmRelations.png){#fig:kdmRelations width=80%}

La Figure \ref{fig:kdmRelations} est le diagramme de classes UIRelations.
Il représente la disposition des UIElements avec les autres et le flux de
    UIDisplay à un autre.

Le flux entre deux Display n'est pas représenté dans nos méta-modèles.
Il définit le comportement de l'interface utilisateur en tant que séquentiel
    flux d'une instance de l'affichage à l'autre.

UILayout définit la présentation d'un AbstractUIElement pour un affichage spécifique.
Cette notion est similaire à notre méta-modèle Layout.

[^KDM]: KDM: Knowledge Discovery Metamodel

### IFML

Brambilla _et al._ [@brambilla2014interaction] a défini le langage IFML (Interaction Flow Modeling Language) pour représenter une application frontale.
Le travail est défini avec l'OMG [(http://www.ifml.org/)](http://www.ifml.org/).
L’IFML a pour but de fournir un système pour décrire la partie vue d’une application,
    avec les composants et les conteneurs, l'état des composants,
    la logique de l'application et la liaison entre les données et l'interface utilisateur.

Avec IFML, un logiciel peut être modélisé avec un ou plusieurs top-containers.
Le conteneur représente une fenêtre principale pour une application de bureau ou une page Web pour une application Web.
Ensuite, chaque conteneur a des sous-conteneurs ou peut contenir des composants.

Un composant est le niveau abstrait d'un widget visible.
Cet élément peut avoir des paramètres d'entrée ou de sortie.
Un paramètre d'entrée d'une ressource de données signifie que le widget affiche la valeur des données.

Le conteneur et le composant peuvent être associés à des événements.
Un élément lié à un événement prend en charge l’interaction des utilisateurs, par exemple, cliquer, glisser-déposer, etc.
Une fois l'action effectuée, l'effet est représenté par une connexion de flux d'interaction qui connecte l'événement
    et les éléments affectés par l'événement.

![IFML View Elements](figures/ifmlViewElements.png){#fig:ifmlViewElements width=80%}

La Figure \ref{fig:ifmlViewElements} présente le meta-model _View Elements_ proposé
    par IFML.
Ce méta-modèle a le même objectif que le modèle d'interface graphique que nous avons conçu, c'est-à-dire représentant le
    partie visible de l'interface utilisateur.
Nous remarquons qu'ils sont légèrement différents.
Les deux utilisent un modèle de conteneur et ont donc la notion de conteneur et de composant,
    respectivement conteneur et feuille dans notre méta-modèle.
Le méta-modèle IFML a introduit la notion de ComponentPart.
Cette entité est nécessaire pour représenter tous les composants possibles car
    les auteurs ont décidé de définir des éléments tels qu'une liste ou un formulaire en tant que composant.
Dans notre cas, une liste est représentée comme un conteneur,
    donc nous n'avons pas besoin d'un ComponentPart parce que nous pouvons utiliser le modèle de conteneur pour
    représente le sous-élément d'un composant.

\newpage