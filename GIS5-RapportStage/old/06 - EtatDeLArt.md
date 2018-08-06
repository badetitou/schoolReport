# Etat de l'art

La première étape de mon travail a été de le positionner par rapport à la littérature existante.

## Migration de plateforme

La _migration de plateforme_ traite de comment migrer une interface graphique.

En particulier, l'article de Samir *et al.* [@samir2007swing2script] dans lequel il est question de la migration de Swing vers Ajax.
Les auteurs ont développé un outil appelé Swing2Script qui permet d'automatiser
  la migration d'application Java-Swing en une application web Ajax.
Cet outil permet d'extraire depuis une application Java les différentes fenêtres et de
  les transcrire au format XUL[^XUL].
Ensuite, l'outil ajoute à chacun des fichiers un fichier JavaScript contenant l'ensemble des fonctions pouvant être appelées.
Cependant, l'outil utilise une analyse dynamique de l'application pour détecter son fonctionnement.
Au vu de la taille des applications de Berger-Levrault ainsi que de l'impact de l'utilisation d'une telle stratégie sur les utilisateurs,
  je ne pourrai pas utiliser la même stratégie.

L'article de Staiger [@staiger2007reverse] présente un outil d'analyse statique de code source en C qui permet d'extraire l'interface graphique générée par le programme.
Pour cela, l'outil va rechercher les constructeurs de composant graphique dans le code et leurs appellent.
Il va ensuite "suivre" les pointeurs (C) pour detecter les ajouts de composants dans d'autres composants.
Ainsi, il arrive à créer un modèle de l'interface graphique avec l'ensemble des widgets.
L'outil permet aussi de récupérer les événements liés aux composants détectés.
Dans la cas d'utilisation d'un modèle pour la migration des applications de Berger-Levrault,
  ce travail peut nous permettre d'extraire le modèle contenant les informations vis-à-vis de la représentation graphique de l'application.

Morgado *et al.* [@morgado2011reverse] ont créé un outil permettant d'extraire les différents composants d'une interface graphique ainsi que les changements qui s'opèrent lorsque l'on effectue une action sur un composant.
Pour cela, les auteurs ont utilisé une stratégie de recherche dynamique.
Une fois lancée sur une interface en cours d'exécution, le logiciel va détecter tous les composants graphiques.
Puis, il va exécuter des cliques sur les composants de l'interface et détecter les modifications apportées à cette dernière.
Comme Berger-Levrault souhaite conserver la même interface graphique,
  je vais aussi avoir besoin d'extraire la structure de l'interface actuelle.
L'outil développait par les auteurs peut peut-être s'appliquer à notre problématique ou nous guider pour la création d'une solution.

L'article de Shah *et al.* [@shah2011reverse] présente un logiciel capable d'extraire l'interface graphique d'une application java pendant l'execution.
Le logiciel récupère dans la mémoire de l'ordinateur l'architecture de l'interface graphique avec la composition des widgets.
De cette manière, les auteurs arrivent à concevoir, pour chaque "écran" de l'application, un modèle contenant les informations sur l'interface graphique.
C'est-à-dire, les différents widgets, leurs propriétés, la composition d'un widget avec un autre (ex: un panel qui contient du text, un autre panel et un bouton).
Comme les auteurs, la migration des applications de Berger-Levrault peut nécessiter que l'on extrait les interfaces graphiques des logiciels.
La stratégie d'extraction présentée dans l'article peut guider notre travail.

Sánchez Ramón *et al.* [@sanchez2014model] ont développé une solution permettant d'extraire depuis un ancien logiciel son interface graphique.
Les auteurs importent cette interface dans un modèle qu'ils ont créé.
Leur solution permet ensuite d'extraire des informations de leur modèle.
Pour cela, l'outil va délimiter pour chaque widget la taille de sa représentation visuelle.
Il connaît donc, la position, la largeur et la hauteur de chaque widget.
Les widgets sont alors contenus ou non dans un autre en fonction de leurs positions les uns avec les autres.
Exemple : un widget qui est à l'intérieur d'un autre visuellement est un contenu par cet autre.
Ce travail se rapproche de celui que nous devons effectuer pour la migration des applications de Berger-Levrault.
Extraire l'interface graphique pour ensuite la migrer fait aussi partie des tâches essentielles de notre migration.
Le modèle représentant l'interface graphique utilisé par les auteurs peut être un point de départ à notre travail d'extraction.

Silvia *et al.* [@silva2010guisurfer] ont créé un logiciel nommé "GUISurfer".
Ce logiciel permet d'extraire une interface graphique d'un code source et de le convertir en un modèle.
Le code source peut être du java avec le framework Swing.
GUISurfer parcourt l'AST[^ast] de l'application source pour détecter les composants qu'on lui a donnés en paramètre.
On peut donc avoir en sortie un modèle complet de l'interface graphique ou ne contenant que les actions (entrée, clique, etc.).
L'article précise qu'un travail sur l'extraction d'interface GWT est en cours.
Pour la migration des applications de Berger-Levrault, je vais devoir extraire l'interface graphique des applications écrites avec GWT.
Le logiciel "GUISurfer" peut peut-être me permettre d'extraire un modèle de l'interface, dans un premier temps sans les spécificités de Berger-Levrault.

L'article de Gotti *et al.* [@gotti2016java] présente une méthodologie pour extraire les composants d'une interface graphique et leurs relations.
Pour cela, les auteurs construisent plusieurs modèles.
Le passage entre les modèles se fait grâce à des transformations QVT[^QVT].
Les auteurs ont appliqué leur projet sur des programmes java utilisant le framework graphique Swing.
Dans le cadre de la migration des applications de Berger-Levrault, l'extraction des composants graphiques
  et de leurs relations peut être nécessaire.
En modifiant le travail des auteurs, je pourrai le réutiliser dans le cas de l'extraction de composant GWT.

Memon *et al.* [@MemonWCRE2003] ont développé un logiciel nommé "GUI Ripper".
Cet outil permet d'extraire d'un logiciel java ou MS Windows les différents composants visuels.
L'outil fait une recherche dynamique des composants instanciés durant l'exécution du programme.
Les auteurs obtiennent un modèle de l'application qu'ils vont ensuite pouvoir utiliser pour générer des tests.
L'extraction de composants visuels est aussi une tâche importante pour mon projet avec Berger-Levrault.
Cependant, la recherche dynamique proposée par les auteurs ne semblent pas applicable en l'état dans notre cas
  puisque nous n'avons pas d'exécution de l'interface, mais sa compilation par GWT.
Cependant, on peut imaginer implémenter l'algorithme de GUI Ripper en JavaScript pour extraire les widgets
  pendant l'exécution du script dans le navigateur web.

L'article de Lelli *et al.* [@lelli2016automatic] propose un outil permettant de détecter les composants graphiques pouvant faire plus de deux actions.
L'outil, qui est une extension d'eclipse, va tout d'abord faire une analyse statique du code source.
Cela lui permet de repérer les différents widgets.
Puis il va détecter les widgets qui ont plus de deux ajout de Listener.
Puisque le plugin est capable de détecter les ajout de listener, il doit être capable de détecter les ajouts d'autres widgets.
Nous pourrions donc modifier le plugin pour qu'il détecte en plus les compositions de widgets afin d'extraire un modèle
  contenant l'aspect graphique des applications de Berger-Levrault.
Cette dernière étape nous sera utile pour migrer correctement les applications.

Cloutier *et al.* [@cloutier2016wavi] ont conçu un outil nommé "Wavi" qui permet d'extraire d'une page web les différents composants.
Pour cela, l'outil se base sur les fichier html, css et JavaScript.
L'outil va dans un premier temps construire l'arbre syntaxique du code source du site web.
Puis il extrait les éléments importants du fichier html (*i.e.* les hyperliens, formulaires, appel JavaScript, etc. ).
Enfin il va relier les éléments de l'étape une et deux.
Les applications web de Berger-Levrault sont développées en java avec GWT, mais une fois compilées sont des
  fichiers html, css et JavaScript.
Toutes les conditions semblent donc remplit pour pouvoir utilise le travail des auteurs dans notre cas et
  ainsi extraire l'architecture des applications de Berger-Levrault.
Ce travail est nécessaire si l'on souhaite conserver la même structure visuelle pendant la migration.

Aho *et al.* [@aho2013industrial] ont développé un logiciel appelé "Murphy".
Murphy permet d'extraire dynamiquement les widgets d'une application.
Murphy est compatible avec de nombreux langages de programmation car il utilise des "drivers" qui lui permettent
  d'interagir avec l'application en cours de fonctionnement grâce à une interface abstraite du langage de programmation de l'application.
Le projet des auteurs se rapprochent du nôtre puisqu'ils souhaitent extraire l'interface graphique
  ce qui est une de nos tâches pour la migration des applications de Berger-Levrault.
Il nous faudrait cependant trouver comment créer un "driver" qui permettrait à Murphy d'interagir avec une application web.

## Migration de librairie

La _migration de librairie_ présente des solutions sur comment changer de framework.

Teyton *et al.* [@teyton2013automatic] ont proposé un outil permettant d'extraire depuis des migrations déjà effectuées
  les correspondances entre les appels d'un framework avec ceux d'un autre.
Pour cela, ils se basent sur les différences textuelles entre plusieurs versions d'un même projet.
À cause du nombre de versions qu'un projet peut avoir, ils utilisent un algorithme
  "diviser pour régner" afin d’accélérer le temps de calcul.
Ce travail peut nous servir une fois que BLCore aura migré, afin d'automatiser ou créer des outils
  facilitant le travail des développeurs.
Cependant, ce travail ne parle que de la migration au sein d'un même langage de programmation.
Il ne peut donc être utilisé tel quel pour faire migrer BLCore de GWT vers Angular 4.

L'article de Hora *et al.* [@hora2015automatic] explique une démarche pour extraire
  automatiquement des modifications faites dans un code afin d'en déduire des pattern.
Son objectif est de détecter les conventions de développement qui évoluent.
Son travail de recherche de correspondance entre certains morceaux de code
  et des nouveaux peut m'être utile pour la migration.
En effet, je vais aussi devoir trouver les correspondances entre du code Java
  et du code Angular.

Zhong *et al.* [@zhong2010mining] ont voulu créer une approche visant à faire correspondre
  l'API[^api] d'un framework avec celui d'un autre, tous deux étant écrits dans des langages différents.
Pour cela, ils ont développé une stratégie appelée MAM (Mining API Mapping) qui prendra en
  paramètres deux projets dans deux versions différentes.
Ensuite, l'algorithme essaie de regrouper par minage les éléments identiques des deux projets
  (_i.e._ les classes, les méthodes).
Puis il construit un arbre d’exécution pour les méthodes et cherche les correspondances
  entre les méthodes qui sont appelées.
Cette approche ne peut pas m'aider à migrer BLCore,
  mais, si je migre quelques applications de Berger-Levrault,
  je pourrai réutiliser les stratégies employées par Zhong pour faciliter la migration d'autres applications.

Nguyen *et al.* [@nguyen2014statistical] ont travaillé sur un outil, nommé StaMiner, permettant de mettre en
  correspondance des API de framework écrit en Java, avec des API écrite en C#.
L'apprentissage des correspondances utilisent des morceaux de code source et cible ayant le même comportement.
Il se fait en trois étapes.

1. Le Groum, qui consiste à représenter le code source sous la forme d'un graphe. On y retrouve les appels à des fonctions, des alternatives, des boucles, etc.
2. L'extraction des séquences d'utilisation des différents éléments.
3. L'alignement des séquences entre le résultat pour le code source et le code cible.

Pour effectuer l'alignement, les auteurs ont utilisé des outils probabilistes sur les symboles et sur les séquences.
Ce travail peut être utilisé pour la migration des applications de Berger-Levrault.
En utilisant cet outil sur une application source et un bout migration *fait à la main*, je pourrai apprendre des règles de transformation à appliquer sur la migration.

L'article de Phan *et al.* [@phan2017statistical] décrit une manière de faire correspondre des éléments de code du langage Java
  vers le langage C#.
Plus précisément, ils ont développé un outil permettant de mettre en correspondance du code d'un langage utilisant
  une ou plusieurs API vers un autre langage utilisant une ou plusieurs autres API prédéfinies.
Pour cela, les auteurs utilisent un outil de recherche de correspondances entre des API provenant de Java vers C#.
Puis, ils utilisent une machine statistique de traduction automatique pour faire correspondre les utilisations des API Java et C#.
Une fois le modèle entraîné, ils arrivent à automatiser une partie de la migration.
Ce travail peut nous servir si l'on a migré BLCore et que l'on souhaite ensuite migrer les applications.
Comme lui, nous travaillons sur la migration de librairies de langages différents.

Chen *et al.* [@chen2016mining] ont développé un outil permettant de trouver des librairies similaires à une autre.
Pour cela, les auteurs ont miné les tags des questions de Stack Overflow.
Avec ces informations, ils ont pu mettre en relation des langages et leurs libraries ainsi que des équivalences entre librairies de langage différent.
Pour la migration de Java/GWT vers Angular, il est possible que nous ayons besoin de changer de librairie (qui n'est pas BLCore).
Plutôt que de récrire la librairie, la recherche d'une autre librairie permettant de résoudre les même problème peut être une solution.
C'est dans ce contexte que le travail des auteurs peut guider notre recherche de librairie en mettant faisant correspondre les anciennes librairies utilisées par les applications de Berger-Levrault avec d'autre compatible utilisable avec Angular.

## Transformation de modèle vers modèle

La _transformation de modèle vers modèle_ traite de la modification d'un modèle source vers un modèle cible.

L'article de Baki *et al.* [@baki2016multi] présente un processus de migration d'un modèle UML vers un modèle SQL.
Pour faire la migration, les auteurs ont décidé d'utiliser des règles de transformation.
Ces règles prennent en entrée le modèle UML et donne en sortie le SQL définit par les règles.
Plutôt que d'écrire les règles de migration à la main.
Les auteurs ont décomposé ces règles en petites briques.
Chaque brique peut correspondre soit à une condition à respecter pour que la règle soit validée, soit à un changement sur la sortie de la règle.
Ensuite, les auteurs ont développé un algorithme de programmation génétique pouvant manipuler ces règles.
L'algorithme va, à partir d'exemples, apprendre les règles de transformation à appliquer afin d'effectuer la transformation du modèle.
Pour cela, il va modifier les petites briques composants les règles et analyser si le modèle en sortie ressemble à
  celui explicité pour tous les exemples.
Puis, il sera utilisé sur de vrais données.
Ce travail peut être utilisé dans mon projet.
En effet, je peux aussi effectuer la migration en utilisant un modèle de l'application source et un
  modèle de l'application cible.
Toutefois, la complexité d'un code source Java semble plus grande que celle d'un modèle UML.
Il est possible que l'algorithme de programmation génétique ne soit pas assez performant pour régler mon problème de manière satisfaisante.

Falleri *et al.* [@falleri2008metamodel] ont travaillé sur le passage d'un méta-modèle à un autre.
C'est ce que l'on appelle l'alignement des méta-modèle.
Pour cela, les auteurs ont utilisé l'algorithme de *Similarity Flooding*.
Cet algorithme permet de trouver les similarités entre les deux graphe orientés et étiquetés aux arcs en entré du programme.
Les auteurs ont proposé des solutions pour convertir un méta-modèle en graphe orientés et étiquetés aux arcs.
Comme les auteurs, je peux décider d'effectuer la migration en passant par des modèles et méta-modèles.
Une fois les méta-modèle et modèles créés, je pourrai utiliser l'algorithme *Similarity Flooding* utilisée dans l'article
  pour effectuer la migration.

Wang *et al.* [@wang2017automatic] ont créé une méthodologie et un outil permettant d'automatiquement faire la transformation d'un modèle vers un autre modèle.
Leur outil se distingue en effectuant une migration qui se base sur une analyse syntaxique et sémantique.
L'objectif de la méthodologie est d'effectuer la transformation d'un modèle vers un autre de manière itérative en modifiant le méta-modèle.
Une condition d'utilisation contraignante décrite par les auteurs est la nécessité d'avoir
  une méta-méta-modèle pour tous les méta-modèle intermédiaire.
Les auteurs ont implémenté un méta-méta-modèle dans leur outil.
Dans mon travail, une solution pour effectuer la migration serait d'utiliser des méta-modèle.
Le premier modèle proviendrait de l'application source, le second modèle respecterait le méta-modèle de destination.
J'ai donc la même problématique que les auteurs de passage d'un modèle à un autre.
En définissant un méta-méta-modèle que respecterai le méta-modèle de départ de l'application source et un méta-modèle de destination,
  la méthodologie proposée par les auteurs devrait pouvoir résoudre, totalement ou partiellement, mon problème de migration.

Fleurey *et al.* [@fleurey2007model] ont travaillé sur la modernisation et la migration de logiciel au sein d'une entreprise d'informatique.
Ils ont développé un logiciel permettant de semi-automatiser la migration des applications de l'entreprise.
Pour cela, ils ont utilisé la transformation de modèle sur trois modèles.
La migration se passe en quatre étapes.

1. Ils génèrent un modèle de l'application à migrer.
2. Ils transforment ce modèle en un "modèle pivot". Ce dernier contient la structure des données, les actions et algorithmes, l'interface graphique et la navigation dans l'application.
3. Ils transforment le modèle pivot en un modèle respectant le méta-modèle du langage cible.
4. Ils génèrent le code source final de l'application.

Comme les auteurs, je vais devoir faire la migration d'une application et je vais devoir conserver structure de données, actions, interface graphique et navigation dans l'application.
Mon travail peut donc s'inspirer de celui proposé par les auteurs si nous souhaitons utiliser les modèles pour effectuer la migration.

L'article de Garcés *et al.* [@garces2017white] décrit les trois étapes que les auteurs ont suivies pour effectuer une migration d'anciens codes.

1. L'analyse de l'ancien code source pour créer un modèle de l'application suivant un méta-modèle. Ce premier modèle est proche de la technologie de départ.
2. La transformation de ce modèle vers un nouveau modèle plus abstrait.
3. La génération du nouveau code source depuis le dernier modèle.

Comme pour l'article, le travail de migration de Berger-Levrault peut se faire en utilisant des modèles et méta-modèles.
De plus, les auteurs ont travaillé sur une migration d'interface graphique ce qui est aussi notre besoin.
Nous pourrions donc réutiliser des outils ou résultats de ce travail dans notre cas.

## Transformation de modèle vers texte

La _transformation de modèle vers texte_ traite du passage d'un modèle source vers du texte.
Le texte peut être du code source compilable ou non.

L'article de Mukherjee *et al.* [@mukherjee2011automatic] présente un outil permettant de prendre en entrée les spécifications d'un programme et donne en sortie un programme utilisable.
L'entrée est un fichier en XML et la sortie est un programme écrit en C ou en Java (en fonction du choix de l'utilisateur).
Pour effectuer les transformations, les auteurs ont utilisé un système de règles de transformation.
Je pourrai réutiliser ce travail si je passe par un modèle pour la migration.
En effet, le fichier XML pris en entrée de l'outil des développeurs peut être assimilé à un modèle suivant un méta-modèle (définit dans l'article).
Dans le cas où nous utilisons un modèle dans le cadre de la migration des applications de Berger-Levrault.
Nous pourrions aussi être amené à utiliser un système de règle pour faire la migration du modèle au code source.

## Migration de langage

La *migration de langage* traite de la transformation du code source directement (*i.e.* sans passer par un modèle).
Pour cela, les auteurs créent des "règles" permettant de modifier le code source.

Brant *et al.* [@brant2010extreme] ont écrit un compilateur utilisant un outil nommé SmaCC.
SmaCC est un générateur d'analyseur pour Smalltalk.
Ils ont aussi utilisé le SmaCC Transformation Toolkit qui permet de définir des règles de transformations qui seront utilisées par SmaCC.
Ainsi, les auteurs sont parvenues à migrer une application Delphi de 1,5 million de lignes de code en C#.
Comme les auteurs, je veux effectuer la migration du code source d'une application.
Mon cas se différencie par les langages source et cible.
Ce travail peut me servir si nous souhaitons effectuer la migration sans passer par un modèle intermédiaire.

Un des problèmes de la migration du code source est la définition des règles.
Newman *et al.* [@newman2017simplifying] ont proposé un outil facilitant la création de règle de transformation.
Pour cela, l'outil va "normaliser" le code source en entré et essayer de le simplifier.
Ainsi, les auteurs arrivent à réduire le nombre de règles de transformations à écrire et leurs complexités.
Dans le cas de migration de Berger-Levrault.
J'aurai à gérer les multiples manières dont les fonctionnalités seront écrites.
La normalisation du code source pourra simplifier l'écriture des règles de transformation ou
  les règles permettant de créer un modèle.

Rolim *et al.* [@rolim2017learning] ont créé un outil qui apprend des règles de transformation de programme à partir d'exemple.
Pour cela, les auteurs ont défini un DSL[^DSL] permettant d'exprimer les modification faîte sur l'AST[^ast] d'un programme.
Ensuite, à partir d'une base d'exemple de transformation, l'outil recherche les règles de transformation entre les fichiers d'entrés des exemples
  et ceux de sorties.
Une fois les règles trouvées et écrites dans le DSL prédéfini, l'outil prend en entré un bout de code et donne en sortie le résultat des transformation.
Ce travail peut nous servir pour la migration des applications de Berger-Levrault.
En effet, nous pouvons imaginer faire la migration de tout ou partie des applications en utilisant un tel outil.
De plus, on peut imaginer un outil qui apprendrai au fur et à mesure des développement des développeur et qui les
  conseillerez dans un second temps sur d'autre développement avec les même problématiques déjà résolu.
Ou encore, l'outil pourrait servir de "guide" pour les nouveaux développeurs participants à la migration ou
  au développement courant des applications.

Aucun des papiers trouvés et cités ne peut nous aider réellement à migrer le framework BLCore ou les applications si on décide que dans le futur, on supprime BLCore, puisqu'Angular, contrairement à GWT n'est pas du Java.
En revanche, si Berger-Levrault souhaite garder l'équivalent de BLCore dans le futur, alors ces travaux pourraient nous aider à migrer dans un deuxième temps les applications.

[^ast]: AST : Arbre Syntaxique Abstrait
[^api]: API : Interface de programmation
[^XUL]: XUL : XML-based User interface Language est un langage de description d’interfaces graphiques fondé sur XML créé dans le cadre du projet Mozilla
[^DSL]: DSL : Domain Specific Language est un langage de programmation destiné à générer des programme dans un domaine spécifique.
[^QVT]: QVT est un langage permettant de définir des transformation de modèle.

\newpage