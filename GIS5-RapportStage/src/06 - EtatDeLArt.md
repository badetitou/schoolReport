# État de l'art

Dans le cadre de la conception de l'outil de migration,
    nous avons étudié la littérature pour identifier
    les techniques respectant les critères définis par Berger-Levrault.
Dans un premier temps, la Section \ref{migrationTechnique} présente les différentes
    techniques qui sont utilisées pour faire effectuer la migration d'application.
Dans un second temps, la Section \ref{positionnement} positionnera notre travail
    par rapport à ceux proposés dans la littérature et utilisant la même technique
    de migration.

## Technique de migration {#migrationTechnique}

Nous avons extrait de la littérature plusieurs domaines de recherche connexes au
    travail que nous voulons mener sur la migration d'application.
Les approchent proposées permettent soit d'effectuer la migration d'application
    sans répondre aux critères que nous avons définis avec Berger-Levrault, soit
    de proposer des pistes à explorer pour créer une solution.

### Rétro-Ingénierie d'interface graphique

La rétro-ingénierie discute de comment représenter une interface graphique
    dans l'objectif de pouvoir l'analyser et comment générer cette représentation.
Le choix de représentation des interfaces graphiques est discuté Section \ref{positionnement}.

Les auteurs utilisent trois techniques pour instancier leurs méta-modèles.

**Statique**. La stratégie statique consiste à analyser le code source de l'application comme on analyserait du texte. 
En fonction du langage source, l'analyse statique peut être plus ou moins facile à mettre en place.

Dans le cas de [@cloutier2016wavi], les auteurs ont pu analyser directement les fichiers HTML, CSS et JavaScript.
Ceci leurs permet de construire un arbre syntaxique du code source du site web
    et d'extraire les différents widgets depuis le fichier HTML.

D'autres auteurs comme [@silva2010guisurfer;@lelli2016automatic;@staiger2007reverse] utilisent un outil qui va analyser du code source provenant de langage non destiné au web, mais permettant de décrire une interface graphique.
Leurs cas d'études sont des applications de bureau ayant une interface graphique.
Ces logiciels vont chercher la définition des widgets dans le code source.
Une fois les créations des widgets trouvés dans le code source, le logiciel va analyser méthodes invoquées ou invoquant les widgets afin de découvrir les relations entre les widgets et leurs attributs.

Sánchez Ramón *et al.* [@sanchez2014model] ont développé une solution permettant d'extraire depuis un ancien logiciel son interface graphique.
Le cas d'étude des auteurs est une application source ayant était créée avec Oracle Forms.
Contrairement aux cas d'études précédents, l'interface est définie dans un fichier à part explicitant pour chaque widget sa position fixe.
Chaque widget a ainsi une position X, Y ainsi qu'une hauteur et une largeur.
La stratégie de l'auteur consiste à déterminer la hiérarchie des widgets depuis ces informations.

**Dynamique**. La stratégie statique consiste à analyser l'interface graphique
    d'une application pendant qu'elle est en fonctionnement.
L'utilisateur va lancer un logiciel dont l'interface est à extraire,
    puis il va lancer l'outil d'analyse dynamique.
Ce dernier va être capable de détecter les différents composants de l'interface et les actions qu'il peut leurs appliquer.
Puis l'outil va appliquer une action sur un élément de l'interface est détecter les changements s'il y en a.
En répétant ces actions, la stratégie va permettre d'explorer les différentes interfaces qui sont utilisées dans l'application à analyser et comment interagir avec ces dernières.

Les auteurs [@MemonWCRE2003;@samir2007swing2script;@shah2011reverse;@morgado2011reverse] ont développé des logiciels implémentant la stratégie dynamique.
Cependant, toutes les solutions proposées s'appliquent sur des applications exécutées sur un ordinateur (application de bureau).
Une adaptation serait nécessaire pour coller aux spécificités des applications web comme dans notre cas.

**Mixe**. L'objectif de la stratégie mixe est d'utiliser la stratégie statique et la stratégie dynamique.
Gotti _et al._ [@gotti2016java] utilise une stratégie mixe pour l'analyse d'applications écrites en Java.
La première étape consiste en la création d'un modèle grâce à une analyse statique du code source.
Les auteurs retrouvent la composition des interfaces graphiques, les différents widgets et leurs propriétés.
Ensuite, l'analyse dynamique va executer les différentes actions possibles sur tous les widgets et
    analyser les modifications potentielles sur l'interface.

### Transformation de modèle vers modèle

La _transformation de modèle vers modèle_ traite de la modification d'un modèle source vers un modèle cible.

L'article de Baki *et al.* [@baki2016multi] présente un processus de migration d'un modèle UML vers un modèle SQL.
Pour faire la migration, les auteurs ont décidé d'utiliser des règles de transformation.
Ces règles prennent en entrée le modèle UML et donne en sortit le SQL définis par les règles.
Plutôt que d'écrire les règles de migration à la main.
Les auteurs ont décomposé ces règles en petites briques.
Chaque brique peut correspondre soit à une condition à respecter pour que la règle soit validée, soit à un changement sur la sortie de la règle.
Ensuite, les auteurs ont développé un algorithme de programmation génétique pouvant manipuler ces règles.
À partir d'exemples l'algorithme apprend les règles de transformation à appliquer afin d'effectuer la transformation du modèle.
Pour cela, il modifie les petites briques composant les règles et analyser si le modèle en sortie ressemble à
  celui explicité pour tous les exemples.
Enfin, l'algorithme est appliqué sur de vraies données.
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
Pour cela, ils ont suivi le principe du _"fer à cheval"_ présenté Figure \ref{horseshoe}.
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
En effet, le fichier XML pris en entrée de l'outil des développeurs peut être assimilé à un modèle suivant un méta-modèle (défini dans l'article).
Dans le cas où nous utilisons un modèle dans le cadre de la migration des applications de Berger-Levrault.
Nous pourrions aussi être amenés à utiliser un système de règle pour faire la migration du modèle au code source.

### Migration de librairie

La _migration de librairie_ présente des solutions sur comment changer de framework.
Ce travail est lié à notre problématique puisque, pour la migration des applications de Berger-Levrault, nous passons de l'utilisation du framework GWT à l'utilisation de frameworks associé à Angular.

Chen *et al.* [@chen2016mining] ont développé un outil permettant de trouver des librairies similaires à une autre.
Pour cela, les auteurs ont miné les tags des questions de Stack Overflow.
Avec ces informations, ils ont pu mettre en relation des langages et leurs librairies ainsi que des équivalences entre librairies de langage différent.
Pour la migration de Java/GWT vers Angular, il est possible que nous ayons besoin de changer de librairie (qui n'est pas BLCore).
Plutôt que de réécrire la librairie, la recherche d'une autre librairie permettant de résoudre les mêmes problèmes peut être une solution.
C'est dans ce contexte que le travail des auteurs peut guider notre recherche de librairie en faisant correspondre les anciennes librairies utilisées par les applications de Berger-Levrault avec d'autres compatibles avec Angular.

### Migration de langage

La *migration de langage* traite de la transformation du code source directement (_c.-à-d._ sans passer par un modèle).
Pour cela, les auteurs créent des "règles" permettant de modifier le code source.

Brant *et al.* [@brant2010extreme] ont écrit un compilateur utilisant un outil nommé SmaCC.
SmaCC est un générateur d'analyseur pour Smalltalk.
Ils ont aussi utilisé le SmaCC Transformation Toolkit qui permet de définir des règles de transformations qui seront utilisées par SmaCC.
Ainsi, les auteurs sont parvenus à migrer une application Delphi de 1,5 million de lignes de code en C#.
Comme les auteurs, je veux effectuer la migration du code source d'une application.
Mon cas se différencie par les langages source et cible.
Ce travail peut me servir si nous souhaitons effectuer la migration sans passer par un modèle intermédiaire.

Un des problèmes de la migration du code source est la définition des règles.
Newman *et al.* [@newman2017simplifying] ont proposé un outil facilitant la création de règles de transformation.
Pour cela, l'outil "normalise" le code source en entré et essayer de le simplifier.
Ainsi, les auteurs arrivent à réduire le nombre de règles de transformations à écrire et leurs complexités.
Dans le cas de migration de Berger-Levrault, je dois gérer les multiples manières dont les fonctionnalités sont écrites.
La normalisation du code source peut simplifier l'écriture des règles de transformation ou
  les règles permettant de créer un modèle.

Rolim *et al.* [@rolim2017learning] ont créé un outil qui apprend des règles de transformation de programme à partir d'exemple.
Pour cela, les auteurs ont défini un DSL[^DSL] permettant d'exprimer les modifications faîtes sur l'AST[^ast] d'un programme.
Ensuite, à partir d'une base d'exemple de transformation, l'outil recherche les règles de transformation entre les fichiers d'entrée des exemples
  et ceux de sorties.
Une fois les règles trouvées et écrites dans le DSL prédéfini, l'outil prend en entrée un bout de code et donne en sortie le résultat des transformations.
Ce travail peut nous servir pour la migration des applications de Berger-Levrault.
En effet, nous pouvons imaginer faire la migration de tout ou partie des applications en utilisant un tel outil.
De plus, on peut imaginer un outil qui apprend au fur et à mesure des nouveaux développements et qui les
  conseillera dans un second temps sur d'autres développements avec les mêmes problématiques déjà résolues.
Ou encore, l'outil pourrait servir de "guide" pour les nouveaux développeurs participants à la migration ou
  au développement courant des applications.

[^ast]: AST : Arbre Syntaxique Abstrait
[^DSL]: DSL : Domain Specific Language est un langage de programmation destiné à générer des programmes dans un domaine spécifique.

## Positionnement sur la migration via les modèles {#positionnement}

Comme décrit Section \ref{sec:processusMigration}, nous avons décidé d'effectuer la migration en utilisant les modèles.
Nous avons utilisé une stratégie selon le _fer à cheval_.

Afin d'appliquer cette stratégie de migration, nous avons dû définir plusieurs méta-modèles décrit Section \ref{sec:metamodelUI} et Section \ref{metamodelComportemental}.
Dans cette Section, nous allons nous positionner par rapport aux modèles proposés dans les autres papiers traitant de la représentation d'une application contenant une interface graphique.
Puis nous présenterons et discuterons les modèles KDM et IFML qui ont été proposés par l'OMG[^OMG] [(https://www.omg.org/spec/KDM/)](https://www.omg.org/spec/KDM/) pour représenter respectivement des applications et des interfaces graphiques.

[^OMG]: OMG : Object Management Group

### Méta-modèle d'interface utilisateur

Nous avons cherché les différences et points communs entre notre méta-modèle d'interface utilisateur et ceux proposés dans la littérature.

[@gotti2016java] ont proposé un méta-modèle inspiré du modèle KDM (voir la Section \ref{sec:kdm}).
Le modèle a les principales entités que nous avons également définies.
On retrouve le patron de conception composite pour représenter le DOM d'une interface graphique.
Leur widget s'appelle _Component_ et comme nous ils ont des propriétés.
Ils ont également implémenté une propriété _Event_ pour le widget.
Mais ils inversent les notions _Event_ et _Action_ comparés à notre solution.

[@fleurey2007model] n'ont pas décrit le méta-modèle de l'interface graphique directement, mais
    nous avons extrait des informations de leur modèle de navigation (voir \ref{sec:navigation}).
Ils ont au moins deux éléments dans leur modèle d'interface graphique, _Window_
    et _GraphicElement_.
_Window_ semble correspondre à notre entité Phase.
On peut supposer que le _GraphicElement_ est un widget.
Le _GraphicElement_ a un _Event_.
Nous n'avons pas la totalité du méta-modèle de l'interface graphique,
    mais nous pouvons voir qu'il ressemble au nôtre.

Le méta-modèle graphique de [@sanchez2014model] est très similaire au notre.
Il y a les entités Widget et Window qui correspondent respectivement à nos entités Widget et Phase.
Leurs widgets ont une position x et y et la largeur et hauteur en tant que propriétés.
Ceci est similaire au lien entre Widgets et Attributes dans notre méta-modèle.
Il y a aussi le patron de conception composite pour représenter le DOM.

[@morgado2011reverse] utilisent un méta-modèle graphique, mais ne le décrivent pas.
Nous savons seulement que l'interface graphique est représentée comme un arbre ce qui est similaire à un
    DOM et peut être représenté grâce au patron de conception composite.

Le méta-modèle graphique de [@garces2017white] diffère beaucoup du notre.
Il y a les attributs, les événements, les windows (qui sont comme les phases),
    mais il n'y a pas de widget.
Cette absence s'explique par la différence dans la technologie source.
Les auteurs ont travaillé sur un projet utilisant des Oracle Forms.
Cette technologie est utilisée pour créer une interface simple avec seulement
    des champs texte ou des formulaires.
Les champs texte contiennent des données provenant d'une base de données.
La disposition des éléments est aussi très simple dans l'exemple fourni
    par les auteurs car les champs texte ou les formulaires sont affichés les uns en dessous des autres.
Nous pouvons encore remarquer qu'ils utilisent une entité _Event_ pour représenter l'action de l'utilisateur 
    avec l'interface utilisateur.

[@MemonWCRE2003] représentent une interface utilisateur graphique avec seulement deux entités dans leur méta-modèle d'interface graphique.
Une GUIWindow similaire à notre phase qui est constituée d'un ensemble de widgets.
Ces widgets peuvent avoir des propriétés et toutes les propriétés ont une valeur associée.
Les auteurs ont défini une interface utilisateur comme ensemble de widgets et leurs propriétés,
    ainsi, si un widget peut avoir deux valeurs différentes pendant l'exécution du programme,
    il appartient à deux interfaces utilisateur différentes.
Ce point est la différence majeure avec les méta-modèles que nous avons proposés car,
    dans notre conception si la valeur d'une propriété change, nous sommes toujours dans la même interface utilisateur,
    mais un code de comportement a été exécuté.

[@samir2007swing2script] ont travaillé sur la migration de l'application Java-Swing vers l'application web Ajax.
Ils ont créé un méta-modèle pour représenter l'interface utilisateur de l'application d'origine.
Ce méta-modèle est stocké dans un fichier XUL et représente
    les widgets avec leurs propriétés ainsi que la mise en page.
Ces widgets appartiennent à une fenêtre, appelée Phase dans notre travail,
    et peuvent déclencher un événement lorsqu'un _Input_ GUI est exécuté.
Dans notre travail, nous avons le même système pour l'événement, mais l'_Input_ est englobé sous le concept d'action.

[@shah2011reverse] utilisent un arbre pour représenter l'interface graphique.
La racine de l'arbre est une _Frame_ ce qui correspond à notre phase.
La racine contient des _Composants_ avec leurs propriétés.
L'entité _Composant_ est similaire à notre entité Widget.

[@joorabchi2012reverse] représentent une interface utilisateur avec un ensemble d'éléments d'interface utilisateur.
Ces éléments sont notre définition d'un widget sans le patron de conception container.
Pour chaque élément d’interface utilisateur, l’outil des auteurs peut gérer la détection
    de plusieurs attributs et de l'événement.

[@memon2007eventflow] utilisent un modèle d'interface graphique pour représenter l'état d'une application (voir \ref{sec:navigation}).
Comme nous, ils ont les widgets avec des propriétés.
L'auteur a fait la différence entre _Widget_ et _Container_,
    il est similaire à l'utilisation du patron de conception container que nous utilisons.
Avec la notion de _Container_, l'auteur est capable de représenter un DOM.

[@mesbah2012crawling] n'ont pas présenté directement le méta-modèle de l'interface utilisateur qu'ils utilisent.
Cependant, ils expliquent qu’ils utilisent une représentation avec un arbre pour
    analyser différentes pages web.
Ils utilisent également la notion d'événement pouvant être déclenchée.
Les auteurs instancient plusieurs méta-modèles d'interface utilisateur pour représenter les différentes pages web de l'application.
Ces instances peuvent être comparées à nos entités Phase.

### Méta-modèle de navigation et de state flow {#sec:navigation}

On retrouve dans la littérature deux autres méta-modèles très présents.
Le méta-modèle de navigation et le méta-modèle de state flow.

Le méta-modèle de navigation permet de représenter un lien entre deux pages web ou fenêtres différentes.
Le lien peut être fait d'une page web à une autre, ou d'un widget vers une page web.
Le méta-modèle de navigation peut aussi contenir un lien d'un widget vers un événement, et un autre de cet événement vers
    une page web.

Morgado _et al._ [@morgado2011reverse] et Fleurey _et al._ [@fleurey2007model] ont utilisé un méta-modèle de navigation
    pour représenter les liens entre les différentes interfaces utilisateur qu'ils détectent.
Les premiers utilisent un méta-modèle supplémentaire qui décrit simplement l'ensemble des _fenêtres_ possible dans l'application.
Son méta-modèle de navigation permet de faire le lien entre une action sur un widget et l'action de navigation qui en résulte.

Pour Fleurey _et al._, le méta-modèle contient directement un lien entre la fenêtre de départ et celle d'arrivée.
Le méta-modèle possède tout de même une entité appelée _Operation_, mais qui ne semble pas impliquée dans la navigation.

Les informations de ces méta-modèles sont complètement contenues dans notre méta-modèle du code comportemental.
Donc, bien que nous n'ayons pas de méta-modèle de navigation, nous faisons au moins ce qui est proposé dans ces papiers.

Le méta-modèle de state flow permet de créer le lien entre différents états de l'interface utilisateur.
Un état de l'interface utilisateur est défini par les widgets visibles et leurs propriétés.
Ainsi, si la valeur d'une propriété change, nous un nouvel état est généré.

[@memon2007eventflow;@mesbah2012crawling;@amalfitano2012using;@silva2010guisurfer;@aho2013industrial] ont tous utilisé un méta-modèle de state flow
    afin de représenter les différentes transitions entre les interfaces graphiques.
Pour définir un état, leurs outils vont analyser les valeurs des propriétés des widgets visibles sur un écran.
Une fois une action exécutée, l'outil va détecter si l'état d'un widget a changé, dans ce cas un nouvel état de l'application est créé.
Ainsi, les auteurs sont capables de représenter les impacts d'une action sur l'interface graphique.

Pour définir un état, Joorabchi _et al._ [@joorabchi2012reverse] ont décidé d'effectuer une comparaison d'image.
Après chaque action sur un widget, exécuté par un outil, ils prennent une image de l'application et la comparent avec une image prise avant l'action.
Si les deux images sont différentes, alors les auteurs ont découvert un changement d'état provenant d'une action.

Le méta-modèle de state flow permet de représenter l'impact d'un événement sur l'interface utilisateur.
Le code à executer sur l'interface afin de passer d'un état à un autre est contenu dans notre méta-modèle du code comportemental.

Notre méta-modèle de code comportemental permet de représenter les informations
    contenues par les méta-modèle de navigation et de state flow.
Cependant, nous ne représentons pas l'état d'entrée et l'état de sortie après une action,
    mais l'état d'entrée d'une fenêtre et la logique à appliquer après une action pour obtenir l'état de sortie.
Cette différence est dû à notre objectif qui est de migrer cette logique dans un nouveau langage et non
    d'analyser les différents états possibles de l'application.

### KDM {#sec:kdm}

L'OMG a défini la norme KDM[^KDM] pour prendre en charge l'évolution des logiciels.
Le standard est un méta-modèle pour représenter un logiciel dans un haut niveau d'abstraction.

![Architecture KDM](figures/kdm.png){#fig:kdm width=80%}

La Figure \ref{fig:kdm} présente un aperçu de l'architecture KDM.
Il est divisé en douze paquetages organisés en quatre couches.
Le paquetage d'interface utilisateur est composé d'un ensemble de méta-modèles pour représenter
    les composants et le comportement de l'interface graphique.
Le paquetage Action définit des méta-modèles pour représenter le comportement d'une application.
Il peut être utilisé pour décrire la logique de l'application, par exemple conditions, boucle, appel.
Il est similaire à notre méta-modèle du code comportemental.
Le paquetage Data représente les données utilisées dans l'application pouvant provenir de services distants ou de bases de données.
Il définit également un méta-modèle représentant les actions que les bases de données peuvent utiliser, par exemple, insertion, mise à jour, suppression.
Ces actions peuvent être exécutées automatiquement lorsqu'un événement se produit.

![KDM Diagramme de Classe UIResources](figures/kdmUI.png){#fig:kdmUI width=80%}

La Figure \ref{fig:kdmUI} est le diagramme de classes UIResources.
Il définit de nombreuses entités pour l'abstraction d'une interface utilisateur.

UIResource peut être défini comme UIDisplay, UIField ou UIEvent.
Un UIField est utilisé pour représenter des formulaires, des champs de texte, des panels, etc.
Il est similaire au Widget de notre méta-modèle d'interface utilisateur.
Nous pouvons également détecter le patron de conception _composite_ avec
    UIResource et AbstractUIElement.
Cela permet la représentation de l'architecture d'interface utilisateur possible.

UIDisplay peut être un Screen ou un Report.
Screen représente une fenêtre pour un logiciel de bureau ou une page web.
Report représente un élément qui sera imprimé.
Cette différence entre Report et Screen n'est pas représentée dans nos méta-modèles.

Chaque AbstractUIElement peut avoir une UIAction.
Une UIAction peut effectuer plusieurs UIEvent.
UIEvent représente les événements qui impactent l'interface utilisateur.
Il peut s'agir d'un _Callback_ ou de l'idée de navigation (_ie._ passer d'une page web à une autre).

[^KDM]: KDM: Knowledge Discovery Metamodel

### IFML

Brambilla _et al._ [@brambilla2014interaction] a défini le langage IFML (Interaction Flow Modeling Language) pour représenter une l'interface graphique d'une application.
Les méta-modèles ont été définis avec l'OMG [(http://www.ifml.org/)](http://www.ifml.org/).
L’IFML a pour but de fournir un système pour décrire la partie visuelle d’une application,
    avec les composants et les conteneurs, l'état des composants,
    la logique de l'application et la liaison entre les données et l'interface utilisateur.

Avec IFML, un logiciel peut être modélisé avec un ou plusieurs conteneurs racines.
Un conteneur représente une fenêtre principale pour une application de bureau ou une page web pour une application web.
Ensuite, chaque conteneur a des sous-conteneurs ou peut contenir des composants.

Un composant est le niveau abstrait d'un widget visible.
Cet élément peut avoir des paramètres d'entrée ou de sortie.
Un paramètre d'entrée d'une décrite comme une donnée signifie que le widget affiche la valeur de la donnée.

Les conteneurs et les composants peuvent être associés à des événements.
Un élément lié à un événement prend en charge l’interaction des utilisateurs, par exemple, cliquer, glisser-déposer, _etc._
Une fois l'action effectuée, l'effet est représenté par une connexion de flux d'interaction qui connecte l'événement
    et les éléments affectés par l'événement.

![IFML View Elements](figures/ifmlViewElements.png){#fig:ifmlViewElements width=80%}

La Figure \ref{fig:ifmlViewElements} présente le méta-modèle _View Elements_ proposé par IFML.
Ce méta-modèle a le même objectif que le modèle d'interface graphique que nous avons conçu.
Il représente la partie visible de l'interface utilisateur.
Nous remarquons qu'ils sont légèrement différents.
Les deux utilisent un patron de conception container et ont donc la notion de conteneur et de composant,
    respectivement conteneur et feuille dans notre méta-modèle.
Le méta-modèle IFML a introduit la notion de ComponentPart.
Cette entité est nécessaire pour représenter tous les composants dans le méta-modèle IFML car
    les auteurs ont décidé de définir des éléments tels qu'une liste ou un formulaire en tant que composant.
Dans notre cas, une liste est représentée comme un conteneur,
    donc nous n'avons pas besoin d'un ComponentPart parce que nous pouvons utiliser le patron de conception container pour
    représenter les sous-éléments d'un composant.

\newpage