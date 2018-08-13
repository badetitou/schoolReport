# État de l'art {#sec:stateOfTheArt}

Dans le cadre de la conception de l'outil de migration,
    nous avons étudié la littérature pour identifier
    les techniques respectant les critères définis par Berger-Levrault.
Dans un premier temps, la \secref{migrationTechnique} présente les différentes
    techniques qui sont utilisées pour faire effectuer la migration d'application.
Dans un second temps, la \secref{positionnement} présentera les différentes
    représentations d'interface graphique que les auteurs de la littérature ont utilisé.

## Technique de migration {#sec:migrationTechnique}

Nous avons extrait de la littérature plusieurs domaines de recherche connexes au
    travail que nous voulons mener sur la migration d'application.
Les approchent proposées permettent soit d'effectuer la migration d'application
    sans répondre aux critères que nous avons définis avec Berger-Levrault, soit
    de proposer des pistes à explorer pour créer une solution.

### Rétro-Ingénierie d'interface graphique

La rétro-ingénierie discute de comment représenter une interface graphique
    dans l'objectif de pouvoir l'analyser et comment générer cette représentation.

Les auteurs utilisent trois techniques pour créer ces représentations : statique, dynamique ou hybride.

**Statique**. La stratégie statique consiste à analyser du code source et à en extraire de l'information.
La stratégie statique n'execute pas le code de l'application à analyser.

Dans le cas de Cloutier _et al._ [@cloutier2016wavi], les auteurs ont analysé directement les fichiers HTML, CSS et JavaScript.
Ceci leurs permet de construire un arbre syntaxique du code source du site web
    et d'extraire les différents widgets depuis le fichier HTML.
Le travail des auteurs ne permet pas de capturer le comportement des éléments graphique.
De plus, le travail consiste surtout en la recherche de lien entre les différents composants
    du programme (classes JavaScript, tag HTML, etc.) et non pas la représentation d'une interface graphique.

Silva _et al._ [@silva2010guisurfer], Lelli _et al._ [@lelli2016automatic] et Staiger _et al._ [@staiger2007reverse] utilisent des outils
    qui analysent du code source provenant de langage non destiné au web, mais permettant de décrire une interface graphique.
Leurs cas d'études sont des applications de bureau ayant une interface graphique.
Ces logiciels cherchent la définition des widgets dans le code source.
Une fois les créations des widgets trouvées dans le code source, les logiciels analysent les méthodes invoquées ou invoquant les widgets afin de découvrir les relations entre les widgets et leurs attributs.
Bien que le travail des auteurs semble intéressant dans le cadre du projet de migration que nous menons,
    les auteurs ne proposent pas de solution sur la manière d'évaluer le résultat de l'outil de rétro-ingénierie sur des applications
    composés de beaucoup d'écran.

Sánchez Ramón *et al.* [@sanchez2014model] ont développé une solution permettant d'extraire depuis un ancien logiciel son interface graphique.
Le cas d'étude des auteurs est une application source créée avec Oracle Forms.
Contrairement aux cas d'études précédents, l'interface est définie dans un fichier à part explicitant pour chaque widget sa position fixe.
Chaque widget a ainsi une position X, Y ainsi qu'une hauteur et une largeur.
La stratégie des auteurs consiste à déterminer la hiérarchie des widgets depuis ces informations.
Cependant, Oracle Forms est utilisée pour créer une interface simple avec seulement des champs texte ou des formulaires.
Les champs texte contiennent des données provenant d'une base de données.
La disposition des éléments est aussi très simple dans l'exemple fourni
    par les auteurs car les champs texte ou les formulaires sont affichés les uns en dessous des autres.
Dans notre cas, les interfaces sont plus complexe et demande une analyse plus poussé que la recherche de la position des widgets.

La stratégie statique permet d'effectuer l'analyse d'une application sans avoir besoin de l'exécuter.
Cependant elle a des lacunes sur l'analyse des structures de contrôles comme les boucles si,
    si elles contiennent des informations à rechercher dans le code.
Dans le cas des interfaces graphiques, il est possible qu'une partie du code soit hébergé par un serveur distant,
    dans ce cas, l'analyse statique n'a pas accès aux résultats des appels au serveur distant (surtout si ceux-ci dépendent de l'appel originel),
    ce qui amène à un manque d'information.

**Dynamique**. La stratégie dynamique consiste à analyser l'interface graphique
    d'une application pendant qu'elle est en fonctionnement.
L'utilisateur lance un logiciel dont l'interface est à extraire,
    puis il lance l'outil d'analyse dynamique.
Ce dernier est capable de détecter les différents composants de l'interface et les actions qu'il peut leur appliquer.
Puis l'outil applique une action sur un élément de l'interface et détecter les changements s'il y en a.
En répétant ces actions, l'approche permet d'explorer les différentes interfaces qui sont utilisées dans l'application à analyser et comment interagir avec ces dernières.

Les auteurs Memon _et al._ [@MemonWCRE2003], Samir _et al._ [@samir2007swing2script], Shah et Tilevich [@shah2011reverse] et Morgado _et al._ [@morgado2011reverse]
    ont développé des logiciels implémentant la stratégie dynamique.
Cependant, toutes les solutions proposées s'appliquent sur des applications exécutées sur un ordinateur (application de bureau).
Une adaptation serait nécessaire pour coller aux spécificités des applications web comme dans notre cas.

L'analyse dynamique permet de parcourir toutes les fenêtres d'une application et d'obtenir des informations provenant de code exécuté.
Cependant, l'analyse impact les performances de l'application à analyser.
Dans le cas de petites applications ou d'application statique, l'impact n'est pas bloquant ou peut être fait en interne.
Mais dans le cadre des applications de Berger-Levrault, l'utilisation de la stratégie dynamique ne peut pas être envisagé car
    l'impact sur les performances pose un problème aux clients de l'entreprise.

**Hybride**. L'objectif de la stratégie hybride est d'utiliser la stratégie statique et la stratégie dynamique.
Gotti _et al._ [@gotti2016java] utilisent une stratégie hybride pour l'analyse d'applications écrites en Java.
La première étape consiste en la création d'un modèle grâce à une analyse statique du code source.
Les auteurs retrouvent la composition des interfaces graphiques, les différents widgets et leurs propriétés.
Ensuite, l'analyse dynamique exécute les différentes actions possibles sur tous les widgets et
    analyse les modifications potentielles sur l'interface après avoir effectué les actions.

Cette stratégie semble permet de collecter un maximum d'information en combinant les avantages
    des stratégies statique et dynamique.
Bien que la stratégie dynamique permet de réduire les problèmes d'analyse de la stratégie statique.
Les contraintes inhérentes à la stratégie dynamique restent présent et important dans le cadre de notre projet.

### Transformation de modèle vers modèle

La _transformation de modèle vers modèle_ traite de la modification d'un modèle source vers un modèle cible.
Dans le cadre d'une migration,
    si nous décidons d'utiliser des modèles pour effectuer des transformation,
    la transformation de modèle est une étape essentielle du processus.

L'article de Baki *et al.* [@baki2016multi] présente un processus de migration d'un modèle UML vers un modèle SQL.
Pour faire la migration, les auteurs ont décidé d'utiliser des règles de transformation.
Ces règles prennent en entrée le modèle UML et donne en sortit le SQL définis par les règles.
Plutôt que d'écrire les règles de migration à la main.
Les auteurs ont décomposé ces règles en petites briques.
Chaque brique peut correspondre soit à une condition à respecter pour que la règle soit validée, soit à un changement sur la sortie de la règle.
Ensuite, les auteurs ont développé un algorithme de programmation génétique pouvant manipuler ces règles.
À partir d'exemples l'algorithme apprend les règles de transformation à appliquer afin d'effectuer la transformation du modèle.
Pour cela, il modifie les petites briques composant les règles et analyse si le modèle en sortie ressemble à
  celui explicité pour tous les exemples.
Enfin, l'algorithme est appliqué sur de vraies données.
Nous avons décidé de ne pas expérimenter cette stratégie de migration.
En effet, dans le cas d'étude des auteurs, il préexiste un certain nombre d'exemples qui favorise l'apprentissage de l'algorithme de machine learning.
Nous ne possédons pas dans notre projet d'exemples et le nombre de déviances dans le code peut vite poser un souci à la formation de l'intelligence artificielle.

\bvc{du coup oui, le méta-méta-modèle c'est comme fame}
Wang *et al.* [@wang2017automatic] ont créé une méthodologie et un outil permettant de faire automatiquement la transformation d'un modèle vers un autre modèle.
Leur outil se distingue en effectuant une migration qui se base sur une analyse syntaxique et sémantique.
L'objectif de la méthodologie est d'effectuer la transformation d'un modèle vers un autre de manière itérative en modifiant le méta-modèle.
Le processus de transformation des auteurs est divisé en quatres étapes.

1. Création de règles de mise en correspondance grâce à une recherche sémantique et
    syntaxique sur les éléments en entrée du processus et ceux désiré en fin de processus.
2. Génération du nouveau modèle grâce aux règles découvertes
3. Evaluation des règles
4. Création des règles au niveau du méta-modèle et génération du nouveau méta-modèle.

Une condition d'utilisation contraignante décrite par les auteurs est la nécessité d'avoir
    un méta-méta-modèle pour tous les méta-modèles intermédiaires.
Le méta-méta-modèle des auteurs définit seulement 9 éléments principaux qui sont
    modèle, élément, noeud, relation entre deux noeuds (_edge_),propriété, primitif, énumération, relation sémantique et relation syntaxique.
Les auteurs ont implémenté ce méta-méta-modèle dans leur outil.
Cette solution permet de définir des règles de transformation et des méta-modèles pour effectuer la transformation de modèle.
Cependant, dans notre cas cela demande un grand nombre d'exemples de code source et de code déjà migré.

![Migration selon le _fer à cheval_](figures/horseshoe.png){#fig:horseshoe width=60%}

Fleurey _et al._ [@fleurey2007model] et Garcés _et al._ [@garces2017white] ont travaillé sur la modernisation et la migration de logiciel.
Ils ont développé des logiciels permettant de semi-automatiser la migration d'applications.
Pour cela, ils ont suivi le principe du _"fer à cheval"_ présenté \figref{horseshoe} proposé par Zang _et al._[@zhang2009migrating].
La migration se passe en quatre étapes.

1. Ils génèrent un modèle de l'application à migrer.
2. Ils transforment ce modèle en un "modèle pivot". Ce dernier contient les structures des données, les actions et algorithmes, l'interface graphique et la navigation dans l'application.
3. Ils transforment le modèle pivot en un modèle respectant le méta-modèle du langage cible.
4. Ils génèrent le code source final de l'application.

Comme les auteurs, nous devons conserver structures de données, actions, interface graphique et navigation dans l'application.
Ce sont des éléments que nous avons représentés dans les méta-modèles que nous avons conçus.
De plus, nous suivons le principe du _"fer à cheval"_.

### Transformation de modèle vers texte

La _transformation de modèle vers texte_ traite du passage d'un modèle source vers du texte.
Le texte peut être du code source compilable ou non.

L'article de Mukherjee *et al.* [@mukherjee2011automatic] présente un outil permettant de prendre en entrée les spécifications d'un programme et donne en sortie un programme utilisable.
L'entrée est un fichier en XML et la sortie est un programme écrit en C ou en Java (en fonction du choix de l'utilisateur).
Pour effectuer les transformations, les auteurs ont utilisé un système de règles de transformation.
Ce travail est lié à notre problématique d'exportation des méta-modèles.
En effet, le fichier XML pris en entrée de l'outil des développeurs peut être assimilé à un modèle suivant un méta-modèle.
Dans ce cas, la génération de code depuis un fichier XML n'est pas très différente de celui de la génération de code depuis des modèles.

### Migration de librairie

La _migration de librairie_ présente des solutions sur comment changer de _framework_.
Ce travail est lié à notre problématique puisque, pour la migration des applications de Berger-Levrault, nous passons de l'utilisation du _framework_ GWT à l'utilisation de frameworks associé à Angular.

Chen *et al.* [@chen2016mining] ont développé un outil permettant de trouver des librairies similaires à une autre.
Pour cela, les auteurs ont miné les tags des questions de Stack Overflow.
Avec ces informations, ils ont pu mettre en relation des librairies avec d'autres librairies quelque soit le langage d'implémentation de ces derniers.
Pour la migration de Java/GWT vers Angular, nous avons besoin de changer de librairie.
Plutôt que de réécrire la librairie, la recherche d'une autre librairie permettant de résoudre les mêmes problèmes peut être une solution.
C'est dans ce contexte que le travail des auteurs peut guider notre recherche de librairie en faisant correspondre les anciennes librairies utilisées par les applications de Berger-Levrault avec d'autres compatibles avec Angular.

La _migration de librairie_ cherche des équivalences entre des librairies,
    pour cela les outils doivent analyser les librairies.
Bien que cela puisse nous aider, la recherche de correspondance entre des librairies n'est pas suffisant pour effectuer la migration,
    la recherche de correspondance peut ne pas être assez précise.
De plus, ce travail ne prend pas en compte les autres détails inhérents à l'application originel.

### Migration de langage

La *migration de langage* traite de la transformation du code source directement (_c.-à-d._ sans passer par un modèle).
Pour cela, les auteurs travaillent sur la création de règles permettant de modifier le code source.

Brant *et al.* [@brant2010extreme] ont développé un outil de définition de règle de transformation.
Ainsi, les auteurs sont parvenus à migrer une application Delphi de 1,5 million de lignes de code en C#.
Comme les auteurs, nous voulons effectuer la migration du code source d'une application.
Notre cas se différencie par les langages source et cible.
Cette solution permettrai de faire la transcription du code GWT vers du code Angular.
Cependant, les auteurs n'ont pas appliqué leurs outils sur un logiciel qui comporte une interface graphique.
Nous ne savons donc pas si la solution est applicable en totalité.

Un des problèmes de la migration du code source est la définition des règles.
Newman *et al.* [@newman2017simplifying] ont proposé un outil facilitant la création de règles de transformation.
Pour cela, l'outil "normalise" le code source en entrée et essaie de le simplifier.
Ainsi, les auteurs arrivent à réduire le nombre de règles de transformations à écrire et leurs complexités.
Dans le cas de migration de Berger-Levrault, nous devons gérer les multiples manières dont les fonctionnalités sont écrites.
La normalisation du code source peut simplifier l'écriture des règles de transformation ou
  les règles permettant de créer les méta-modèles.

Rolim *et al.* [@rolim2017learning] ont créé un outil qui apprend des règles de transformation de programme à partir d'exemples.
Pour cela, les auteurs ont défini un DSL[^DSL] permettant d'exprimer les modifications faites sur l'AST[^ast] d'un programme.
Ensuite, à partir d'une base d'exemple de transformation, l'outil recherche les règles de transformation entre les fichiers d'entrée des exemples
  et ceux de sorties.
Une fois les règles trouvées et écrites dans le DSL prédéfini, l'outil prend en entrée un bout de code et donne en sortie le résultat des transformations.
Ce travail peut nous servir pour la migration des applications de Berger-Levrault.
En effet, nous pouvons imaginer faire la migration de tout ou partie des applications en utilisant un tel outil.
Par exemple, une fois la migration semi-automatique effectuée par la solution que nous avons proposée,
    ce type d'outil pourrait servir de "guide" pour les nouveaux développeurs participants à la migration ou au développement courant des applications.

[^ast]: AST : Arbre Syntaxique Abstrait
[^DSL]: DSL : Domain Specific Language est un langage de programmation destiné à générer des programmes dans un domaine spécifique.

## Représentation d'interface graphique dans la littérature {#sec:positionnement}

Nous avons vu dans la Section précédente que la représentation abstraite des interfaces graphiques est souvent utilisé.
Nous avons donc recherché et comparé les différentes représentations existantes.

\secref{omg}, nous présentons les deux ensemble de méta-modèles défini par l'OMG[^OMG] pour représenter une application.
Le premier KDM permet de représenter une application de manière générale tandis que le second, IFML, est spécialisé dans la
    représentation d'application ayant une interface graphique.
La \secref{stateMetaUI} détaille les représentations des interfaces graphiques décrites dans la littérature.

[^OMG]: OMG : Object Management Group

### Standard défini par l'OMG {#sec:omg}

L'OMG a défini la norme KDM[^KDM] [(https://www.omg.org/spec/KDM/)](https://www.omg.org/spec/KDM/) pour prendre en charge l'évolution des logiciels.
Le standard utilise un méta-modèle pour représenter un logiciel dans un haut niveau d'abstraction.

L'architecture KDM est divisée en douze paquetages organisés en quatre couches.
Le paquetage d'interface utilisateur est composé d'un ensemble de méta-modèles pour représenter
    les composants et le comportement de l'interface graphique.
Le paquetage Action définit des méta-modèles pour représenter le comportement d'une application.
Il peut être utilisé pour décrire la logique de l'application, par exemple conditions, boucle, appel.
Le paquetage Data représente les données utilisées dans l'application pouvant provenir de services distants ou de bases de données.
Il définit également un méta-modèle représentant les actions que les bases de données peuvent utiliser, par exemple, insertion, mise à jour, suppression.
Ces actions peuvent être exécutées automatiquement lorsqu'un événement se produit.

![KDM Diagramme de Classe UIResources](figures/kdmUI.png){#fig:kdmUI width=80%}

La \figref{kdmUI} est le diagramme de classes UIResources.
Il définit de nombreuses entités pour l'abstraction de l'interface utilisateur.

UIResource peut être défini comme UIDisplay, UIField ou UIEvent.
Un UIField est utilisé pour représenter des formulaires, des champs de texte, des panels, etc.
Nous pouvons également détecter le patron de conception _composite_ avec
    UIResource et AbstractUIElement.
Cela permet la représentation de l'architecture d'interface utilisateur possible.

UIDisplay peut être un Screen ou un Report.
Screen représente une fenêtre d'un logiciel de bureau ou une page web.
Report représente un document qui sera imprimé.

Chaque AbstractUIElement peut avoir une UIAction.
Une UIAction peut effectuer plusieurs UIEvent.
UIEvent représente les événements qui impactent l'interface utilisateur.
Il peut s'agir d'un _Callback_ ou de l'idée de navigation (_c.-à-d._ passer d'une page web à une autre).

[^KDM]: KDM: Knowledge Discovery Metamodel

Le langage IFML proposé par Brambilla _et al._ [@brambilla2014interaction] et adopté par l'OMG permet de représenter une l'interface graphique d'une application.
Les méta-modèles ont été définis avec l'OMG [(http://www.ifml.org/)](http://www.ifml.org/).
L’IFML a pour but de fournir un système pour décrire la partie visuelle d’une application,
    avec les composants et les conteneurs, l'état des composants,
    la logique de l'application et la liaison entre les données et l'interface utilisateur.

Avec IFML, un logiciel peut être modélisé avec un ou plusieurs conteneurs racines.
Un conteneur représente une fenêtre principale pour une application de bureau ou une page web pour une application web.
Ensuite, chaque conteneur a des sous-conteneurs ou peut contenir des composants.

Un composant est le niveau abstrait d'un widget visible.
Cet élément peut avoir des paramètres d'entrée ou de sortie.
Un paramètre d'entrée décrite comme une donnée signifie que le widget affiche la valeur de la donnée.

Les conteneurs et les composants peuvent être associés à des événements.
Un élément lié à un événement prend en charge l’interaction des utilisateurs, par exemple, cliquer, glisser-déposer, _etc._
Une fois l'action effectuée, l'effet est représenté par une connexion de flux d'interaction qui connecte l'événement
    et les éléments affectés par l'événement.

![IFML View Elements](figures/ifmlViewElements.png){#fig:ifmlViewElements width=100%}

La Figure \ref{fig:ifmlViewElements} présente le méta-modèle _View Elements_ proposé par IFML.
Ce méta-modèle a pour objectif de représenter la partie visible de l'interface utilisateur.
Il utilise le patron de conception container et ont donc la notion de conteneur et de composant afin de représenter le DOM.
Le méta-modèle IFML introduit aussi la notion de ComponentPart.
Cette entité est nécessaire pour représenter tous les composants dans le méta-modèle IFML car
    les auteurs ont décidé de définir les éléments tels que les listes ou les formulaires en tant que composant.
Ils ne peuvent donc pas contenir d'autres éléments, ce qui reviendrait à n'avoir que des listes, tableaux et formulaires vides.
L'utilisation d'un ComponentPart permet d'ajouter d'autre composant aux listes et formulaire sans pour autant les considérer
    comme présent dans le DOM mais plus comme des données appartenant aux composants.

### Méta-modèle d'interface utilisateur {#sec:stateMetaUI}

Nous présentons dans cette Section les méta-modèles ou représentation d'interface graphique proposés dans la littérature.
Nous comparons ces propositions avec ceux proposés par l'OMG.

Gotti _et al._[@gotti2016java] ont proposé un méta-modèle inspiré du modèle KDM (voir la \secref{sec:omg}).
Le méta-modèle a les principales entités défini dans le modèle KDM.
On retrouve le patron de conception composite pour représenter le DOM d'une interface graphique.
La notion de UIElement s'appelle _Components_.
Les components comme les fenêtres ont une notion de _Property_ qui a été ajouté par les développeurs
    pour représenter des informations comme, par exemple, la couleur d'un bouton.
Ils ont également implémenté une propriété _Event_ pour les components.
Mais ils inversent les noms des notions _Event_ et _Action_ comparées aux modèles KDM.

Fleurey _et al._[@fleurey2007model] n'ont pas décrit le méta-modèle de l'interface graphique directement, mais
    nous avons extrait des informations de leur méta-modèle de navigation.
Ils ont au moins deux éléments dans leur méta-modèle d'interface graphique, _Window_ et _GraphicElement_.
_Window_ semble correspondre à la notion de Display du méta-modèle KDM et au conteneur racine de IFML.
On peut supposer que le _GraphicElement_ est un UIRessource.
Le _GraphicElement_ a un _Event_.
Nous n'avons pas la totalité du méta-modèle de l'interface graphique,
    mais nous pouvons voir qu'il ressemble à ceux proposés par l'OMG.

Le méta-modèle graphique de Sanchez _et al._[@sanchez2014model] est très à ceux de l'OMG.
Il y a les entités Widget et Window qui correspondent respectivement aux entités GUIElement et UIDisplay.
Leurs widgets ont une position X et Y et la largeur et hauteur en tant que propriétés.
Ceci n'est pas représenté dans les méta-modèles KDM et IFML.
Il y a aussi le patron de conception composite pour représenter le DOM.

Morgado _et al._[@morgado2011reverse] utilisent un méta-modèle graphique, mais ne le décrivent pas.
Nous savons seulement que l'interface graphique est représentée comme un arbre ce qui est similaire à un
    DOM et peut être représenté grâce au patron de conception composite.

Le méta-modèle graphique de Garces _et al._[@garces2017white] diffère beaucoup de ceux précédemment décrit.
Il y a les attributs, les événements, les windows, mais il n'y a pas de widget.
Cette absence s'explique par la différence dans le langage source à migrer.
Les auteurs ont travaillé sur un projet utilisant des Oracle Forms.
L'interface est décrite dans des fichier à part et
    est souvent composé d'affichage d'éléments d'une base de donnée plutôt que de widget.
Nous pouvons tout de même remarquer qu'ils utilisent une entité _Event_ pour représenter l'action de l'utilisateur
    avec l'interface utilisateur.

Memon _et al._[@MemonWCRE2003] représentent une interface utilisateur graphique avec seulement deux entités dans leur méta-modèle d'interface graphique.
Une GUIWindow similaire au UIDisplay qui est constituée d'un ensemble de widgets.
Ces widgets peuvent avoir des propriétés et toutes les propriétés ont une valeur associée.
Les auteurs ont défini une interface utilisateur comme ensemble de widgets et leurs propriétés,
    ainsi, si un widget peut avoir deux valeurs différentes pendant l'exécution du programme,
    il appartient à deux interfaces utilisateur différentes.
Ce point est la différence majeure avec les méta-modèles proposés par l'OMG car,
    dans la conception proposé par IFML si la valeur d'une propriété change,
    nous sommes toujours dans la même interface utilisateur,
    mais un flux d'interaction a été exécuté.

Smair _et al._[@samir2007swing2script] ont travaillé sur la migration d'une application Java-Swing vers une application web Ajax.
Ils ont créé un méta-modèle pour représenter l'interface utilisateur de l'application d'origine.
Ce méta-modèle est stocké dans un fichier XUL et représente
    les widgets avec leurs propriétés ainsi que la mise en page.
Ces widgets appartiennent à une fenêtre, appelée Phase dans notre travail,
    et peuvent déclencher un événement lorsqu'un _Input_ GUI est exécuté.
Dans les modèles de l'OMG, on ne retrouve pas la notion de propriété et
    l'_Input_ est englobé soit dans paquetage _Event_ pour KDM, soit dans flux d'interaction pour IFML.

Shah et Tilevich[@shah2011reverse] utilisent un arbre pour représenter l'interface graphique.
La racine de l'arbre est une _Frame_ ce qui correspond à la notion de _UIDisplay_.
La racine contient des _Composants_ avec leurs propriétés.
L'entité _Composant_ est similaire à l'entité _UIField_
    bien que les propriétés ne sont pas représentées dans les méta-modèles KDM.

Joorachi _et al._[@joorabchi2012reverse] représentent une interface utilisateur avec un ensemble d'éléments d'interface utilisateur.
Ces éléments correspondent à la notre définition d'un UIField.
Pour chaque élément d’interface utilisateur, l’outil des auteurs peut gérer la détection
    de plusieurs attributs et d'événements.
Les attributs sont absents des modèles proposés par l'OMG.
On ne retrouve pas le patron de conception _container_ dans le travail de ces auteurs.

Memon _et al._[@memon2007eventflow] utilisent un modèle d'interface graphique pour représenter l'état d'une application.
Ils ont aussi utilisé la notion de UIField.
Les auteurs utilise le patron de conception _container_ afin de représenter le DOM d'une application.

Mesbah _et al._[@mesbah2012crawling] n'ont pas présenté directement le méta-modèle de l'interface utilisateur qu'ils utilisent.
Cependant, ils utilisent une représentation avec un arbre pour analyser différentes pages web.
Ils utilisent également la notion d'événement pouvant être déclenchée.
Les auteurs instancient plusieurs méta-modèles d'interface utilisateur pour représenter les différentes pages web de l'application.
Ces instances peuvent être comparées à plusieurs éléments UIDisplay.

Nous retrouvons dans les papiers la représentation du DOM grâce à avec un arbre ou un méta-modèle utilisant le patron de conception composite.
Les notions de UIDisplay est aussi omniprésente.
Contrairement à ce qui est utilisé dans les méta-modèles KDM et IFML, l'utilisation d'une entité _Attribut_ est souvent utilisé.

Maintenant que nous avons étudié comment les interfaces graphiques sont représenté dans la littérature et
    comment ces représentations sont construites.
Il faut que l'on détaille les couches composants une interface graphique.
