# État de l'art

La première étape de mon travail a été de le positionner par rapport à la littérature existante.

## Migration de plateforme

La _migration de plateforme_ traite de comment migrer une application d'un langage à un autre.
On y aborde par exemple la migration de plateforme pour des interfaces graphiques.
En particulier, l'article de Samir *et al.* [@samir2007swing2script] dans lequel il est question de la migration de Swing vers Ajax.
Les auteurs ont développé un outil appelé Swing2Script qui permet d'automatiser
  la migration d'application Java-Swing en une application web Ajax.
Cet outil permet d'extraire depuis une application Java les différentes fenêtres et de
  les transcrire au format XUL[^XUL].
Ensuite, l'outil ajoute à chacun des fichiers un fichier JavaScript contenant l'ensemble des fonctions pouvant être appelées.
Cependant, l'outil utilise une analyse dynamique de l'application pour détecter son fonctionnement.
Au vu de la taille des applications de Berger-Levrault ainsi que de l'impact de l'utilisation d'une telle stratégie sur les utilisateurs,
  je ne pourrai pas utiliser la même stratégie.


## Migration de librairie

La _migration de librairie_ présente des solutions sur comment changer de framework.

Teyton  *et al.* [@teyton2013automatic] ont proposé un outil permettant d'extraire depuis des migrations déjà effectuées
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

Zhong  *et al.* [@zhong2010mining] ont voulu créer une approche visant à faire correspondre
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

L'article de Phan  *et al.* [@phan2017statistical] propose de faire correspondre des éléments de code du langage Java
  vers le langage C#.
Plus précisément, ils ont développé un outil permettant de mettre en correspondance du code d'un langage utilisant
  une ou plusieurs API vers un autre langage utilisant une ou plusieurs autres API prédéfinies.
Pour cela, les auteurs utilisent un outil de recherche de correspondances entre des API provenant de Java vers C#.
Puis, ils utilisent une machine statistique de traduction automatique pour faire correspondre les utilisations des API Java et C#.
Une fois le modèle entraîné, ils arrivent à automatiser une partie de la migration.
Ce travail peut nous servir si l'on a migré BLCore et que l'on souhaite ensuite migrer les applications.
Comme lui, nous travaillons sur la migration de librairies de langages différents.

Aucun des papiers trouvés et cités ne peut nous aider réellement à migrer le framework BLCore ou les applications si on décide que dans le futur, on supprime BLCore, puisqu'Angular, contrairement à GWT n'est pas du Java.
En revanche, si Berger-Levrault souhaite garder l'équivalent de BLCore dans le futur, alors ces travaux pourraient nous aider à migrer dans un deuxième temps les applications.

[^api]: API: Interface de programmation
[^XUL]: XUL: XML-based User interface Language est un langage de description d’interfaces graphiques fondé sur XML créé dans le cadre du projet Mozilla

[//]: # \newpage
