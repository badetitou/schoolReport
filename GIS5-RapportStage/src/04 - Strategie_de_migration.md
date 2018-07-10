# Description du problème

Dans le contexte de l'évolution des applications de Berger-Levrault,
    l'entreprise a estimé le transformation du code à X jours de développement.
Ceci s'explique majoritairement par les X MLOC[^mloc] utilisés pour les logiciels.
Un de mes objectifs à Berger-Levrault est de définir une stratégie de migration
    qui réduit le temps nécessaire pour la transformation des programmes.

[^mloc]: MLOC : Million lines of code

## Contraintes {#contraintes}

Berger-Levrault étant une importante entreprise dans le domaine de l'édition de logiciel,
    elle a des contraintes spécifiques vis-à-vis d'un outil de migration.
En effet, la solution logiciel que j'ai produit doit respecter les contraintes suivantes :

- _Indépendance du langage source_. La solution doit être facile à adapter pour tout projets utilisant GWT ainsi que d'autre logiciel n'étant pas écrit en JAVA mais possédant une interface utilisateur. Cette contrainte doit être respecté pour pouvoir être réutiliser sur différents projet. Dans le cas de Berger-Levrault, cela permet aux développeurs d'appliquer la solution sur d'autres logiciels qu'ils veulent migrer.
- _Indépendance du langage cible_. Il doit être facile de changer le langage cible de la migration sans devoir restructurer ou développer l'implémentation de la stratégie de migration. Cette contrainte garantie que la solution peut être utilisée quelque soit l'architecture du langage cible.
    Dans notre cas, nous migrons des applications de GWT vers Angular. Angular a vu 6 versions majeurs sortir depuis 2016. Grâce à l'indépendance du langage cible, la stratégie de migration reste valide quelque soit la version du langage cible.
- _Approche modulaire_. La migration doit être divisée en petite étapes. Cela permet de facilement remplacer une étapes ou de l'étendre sans introduire d'instabilité. Cette contrainte est essentielle pour les entreprises qui désirent avoir un contrôle fin du processus de migration. L'approche modulaire permet entre autres aux entreprises de modifier l'implementation de la stratégie pour respecter leurs contraintes spécifiques.
- _Préservation de l'architecture_. Après la migration, nous devons retrouver la même architecture entre les différents composants de l'interface graphique (_c.-à-d._ un bouton qui appartenait à un panel dans l'application source appartiendra au même panel dans l'application cible). Cette contrainte permet de faciliter le travail de comprehension de l'application cible par les développeurs. En effet, ils vont retrouver la même architecture qu'ils avaient dans l'application source.
- _Préservation du visuel_. Il ne doit pas y avoir de différence visuelle entre l'application source et l'application cible. Cette contrainte est particulièrement importante pour les logiciels commerciaux. En effet, les utilisateurs de l'application ne doivent pas être perturbés par la migration.

Une dernière contrainte inhérent aux entreprises est la possibilité pour les équipes de développement de continuer la maintenance des applications pendant le développement de la stratégie de migration et la migration elle même.

## Comparaison de GWT et Angular

 |  |    GWT |   Angular
------------------ | ------------------ | -----------------
  page web | Une classe Java | Un fichier TypeScript et un fichier HTML
   style pour une page web | Inclue dans le fichier Java | un fichier CSS optionnel
  Nombre de fichier de configuration | Un fichier de configuration |  Quatre fichiers plus deux par sous-projets

: Comparaison des architectures de GWT et Angular. \label{comparaison}

Dans le cas de ce projet, le langage de programmation source et cible ont deux architectures différentes.
Les différences sont syntaxical, semantical et architectural.
Pour la migration d'application GWT vers Angular, les fichier _.java_ sont séparé en plusieurs fichiers Angular.

Comme présenté dans le Tableau \ref{comparaison}, la séparation des fichiers Java en fichier
    Angular se fait à trois endroits, les fichiers de configuration, les fichiers définissant la page web et les fichiers de style.

Avec le Framework GWT, un seul fichier est nécessaire pour représenter une page web.
L'ensemble de la page web peut donc être contenu dans ce fichier,
    il reste toutefois possible de créer d'autre fichier pour séparé les différents éléments de la page web.
Le fichier java contient les différents widgets de la page web, leurs positions les uns par rapport aux autres et leurs organisations hiérarchique.
Dans le cas de widget sur lesquels une action peut être exécuté (comme un bouton), c'est dans ce
    même fichier qu'est contenu le code à executer lorsque l'action est réalisée.
En Angular, on crée une hiérarchie de fichier correspondant à un _sous-projet_ pour chaque page web.
Ce sous-projet contient plusieurs fichiers dont un fichier HTML qui contient les widgets de la page web et leurs organisations, et un fichier TypeScript contenant le code à exécuter quand une action ce produit
sur les widgets du fichier HTML.
On a donc une séparation de un fichier GWT vers deux fichiers Angular pour représenter les widgets et le code qui leurs est associé.

Pour le style visuel d'une page web, dans le cas de GWT, il y a un fichier CSS commun à toutes les pages webs et des modifications qui sont appliquées directement dans le fichier java de la page web. Ces modifications peuvent porter sur la couleur ou les dimensions.
En Angular, on retrouve le même fichier CSS général pour tout le projet,
    cependant c'est un fichier CSS que l'on doit créer par sous-projet qui va définir le visuel des éléments de la page web.
Il y a donc création d'un fichier supplémentaire en Angular par rapport à GWT.

Pour les fichiers de configurations, GWT n'a besoin que d'un fichier de configuration qui défini
    les fichiers java correspondant à une page web et les URL que l'on devra utiliser pour y accéder.
En Angular, il y a deux fichiers de configuration générales. Le premier, _module_, explicite les différentes page web accessible dans l'application ainsi que les services distant et les composants graphiques (widgets) utilisable dans l'application. Le second, de _routing_, définit pour les différentes pages web de l'application leurs chemins d'accès.

## Stratégies de migration

Il existe plusieurs manières d'effectuer la migration d'une application.
Toute les solutions doivent respecter les contraintes définis
    Section \ref{contraintes}.

![Exemple de règle de transformation](figures/exampleRuleEngine.png){#exampleRuleEngine width=350px height=250px}

- _Migration manuelle_. Cette stratégie correspond au re-développement complet des applications sans l'utilisation d'outils aidant à la migration. La migration manuelle permet de facilement corriger les potentielles erreurs de l'application d'origine et de re-concevoir l'application cible en suivant les precepts du langage cible.  
- _Utilisation d'un moteur de règle_. L'utilisation d'un moteur de règle pour migrer partiellement ou en totalité une application a déjà été appliqué sur d'autres projet [@brant2010extreme; @feldman1990fortran; @grosse2012automatic]. Pour utiliser cette stratégie, nous devons définir et créer des règles qui prennent en entré le code source et qui produisent le code pour l'application migrée. La Figure \ref{exampleRuleEngine} montre un exemple de règle de transformation. Dans ce cas, elle permet de changer la position des opérateurs dans une expression mathématique source. L'opérateur est maintenant en suffixe de l'expression. Il est possible que la migration ne soit pas complète. Dans ce cas, les développeurs devront finir le processus de migration avec du travail manuelle. L'utilisation d'un moteur de règle, bien qu'efficace, implique une solution qui n'est ni indépendante de la source, ni indépendante de la cible de la migration.
- _Migration dirigée par les modèles_. La migration dirigée par les modèles implique le développement de méta-modèles pour effectuer. La stratégie respecte l'ensemble des contraintes que nous avons défini. Une migration semi-automatique ou complètent automatique est envisageable avec cette stratégie de migration. Comme pour l'utilisation d'un moteur de règle, dans le cas d'une migration semi-automatique, il peut y avoir du travail manuel à effectuer pour compléter la migration.

\newpage