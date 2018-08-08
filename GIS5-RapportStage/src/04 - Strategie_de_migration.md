# Description du problème

Dans le contexte de l'évolution des applications de Berger-Levrault,
    l'entreprise a estimé la transformation du code à X jours de développement.
Ceci s'explique majoritairement par les 1,67 MLOC[^mloc] utilisés pour les logiciels.
Un de mes objectifs à Berger-Levrault est de définir une stratégie de migration
    qui réduit le temps nécessaire pour la transformation des programmes.

[^mloc]: MLOC : Million lines of code

## Contraintes {#contraintes}

Berger-Levrault étant une importante entreprise dans le domaine de l'édition de logiciel,
    elle a des contraintes spécifiques vis-à-vis d'un outil de migration.
En effet, la solution logicielle que j'ai produite doit respecter les contraintes suivantes :

- _Depuis GWT (BLCore)_. La solution doit au moins fonctionner dans le cas de Berger-Levrault. Elle peut être plus générale, mais ne doit pas faire de concession sur le résultat final.
- _Vers Angular_. Dans le cas d'une automatisation ou semi-automatisation du processus de migration, celle-ci doit s'achever par la génération de code Angular. La solution peut contenir des structures facilitant son utilisation pour d'autres langages cibles mais pas au détriment du projet fixé avec Berger-Levrault.
- _Approche modulaire_. La migration doit être divisée en petites étapes. Cela permet de facilement remplacer une étape ou de l'étendre sans introduire d'instabilité. Cette contrainte est essentielle pour les entreprises qui désirent avoir un contrôle fin du processus de migration. L'approche modulaire permet entre autres aux entreprises de modifier l'implémentation de la stratégie pour respecter leurs contraintes spécifiques.
- _Préservation de l'architecture_. Après la migration, nous devons retrouver la même architecture entre les différents composants de l'interface graphique (_c.-à-d._ un bouton qui appartenait à un panel dans l'application source appartiendra au même panel dans l'application cible). Cette contrainte permet de faciliter le travail de compréhension de l'application cible par les développeurs. En effet, ils vont retrouver la même architecture qu'ils avaient dans l'application source.
- _Préservation du visuel_. Il ne doit pas y avoir de différence visuelle entre l'application source et l'application cible. Cette contrainte est particulièrement importante pour les logiciels commerciaux. En effet, les utilisateurs de l'application ne doivent pas être perturbés par la migration.
- _Automatique_. La solution apportée doit être automatique. Les utilisateurs de l'outil ne devraient pas intervenir pendant le processus de migration ou très peu. Ainsi, l'outil peut être utilisé avec un minimum de connaissance préalable.
- _Amélioration de la qualité_. La migration doit permettre de traiter les possibles déviances du programme source. Par exemple, dans le cas de Berger-Levrault, l'outil de migration doit être capable de gérer les éléments utilisés par l'application à migrer et provenant du framework GWT. Cet exemple d'utilisation du framework GWT par l'Application 1 est représenté Figure \ref{architectureBL}.

Une dernière contrainte inhérente aux entreprises est la possibilité pour les équipes de développement de continuer la maintenance des applications pendant le développement de la stratégie de migration et la migration elle-même.

## Comparaison de GWT et Angular

\begin{table}[hbtp]
    \begin {center}
    \resizebox{\columnwidth}{!}{%
    \begin{tabular}{|l|l|l|}
        \hline
         & GWT & Angular \\
        \hline
        Page web    & Une classe Java & Un fichier TypeScript et un fichier HTML \\
        \hline
        Style pour une page web & Inclus dans le fichier Java & Un fichier CSS optionnel \\
        \hline
        Nombre de fichiers de configuration & Un fichier de configuration & Quatre fichiers plus deux par sous-projets \\
        \hline
    \end{tabular} %
    }
    \end{center}
    \caption{Comparaison des architectures de GWT et Angular}
    \label{comparaison}
\end{table}

Dans le cas de ce projet, le langage de programmation source et cible ont deux architectures différentes.
Les différences sont syntaxicales, semanticales et architecturales.
Pour la migration d'application GWT vers Angular, les fichiers _.java_ sont séparés en plusieurs fichiers Angular.

Le Tableau \ref{comparaison} synthétise les différences entre l'architecture d'une application en java et celle en Angular.
Les différences se font pour trois notions, les pages web, leurs styles et les fichiers de configuration.

Avec le Framework GWT, un seul fichier est nécessaire pour représenter une page web.
L'ensemble de la page web peut donc être contenu dans ce fichier,
    il reste toutefois possible de créer d'autres fichiers pour séparer les différents éléments de la page web.
Les fichiers java contiennent les différents widgets de la page web, leurs positions les uns par rapport aux autres et leurs organisations hiérarchiques.
Dans le cas d'un widget sur lequel une action peut être exécutée (comme un bouton), c'est dans ce
    même fichier qu'est contenu le code à exécuter lorsque l'action est réalisée.
En Angular, on crée une hiérarchie de fichier correspondant à un _sous-projet_ pour chaque page web.
Ce sous-projet contient plusieurs fichiers dont un fichier HTML qui contient les widgets de la page web et leurs organisations, et un fichier TypeScript contenant le code à exécuter quand une action se produit sur les widgets du fichier HTML.
On a donc la séparation d'un fichier java pour GWT vers deux fichiers HTML et TypeScript en Angular pour représenter les widgets et le code qui leur sont associés.

Pour le style visuel d'une page web, dans le cas de GWT, il y a un fichier CSS commun à toutes les pages web et des modifications qui sont appliquées directement dans le fichier java de la page web.
Ces modifications peuvent porter sur la couleur ou les dimensions.
En Angular, on retrouve le même fichier CSS général pour tout le projet,
    cependant, c'est un fichier CSS que l'on doit créer par sous-projet qui va définir le visuel des éléments de la page web.
Il y a donc création d'un fichier supplémentaire en Angular par rapport à GWT.

Pour les fichiers de configurations, GWT n'a besoin que d'un fichier de configuration qui définit
    les fichiers java correspondant à une page web et les URL que l'on devra utiliser pour y accéder.
En Angular, il y a deux fichiers de configuration générale. Le premier, _module_, explicite les différentes pages web accessibles dans l'application ainsi que les services distants et les composants graphiques (widgets) utilisables dans l'application. Le second, de _routing_, définit pour les différentes pages web de l'application leurs chemins d'accès.

## Stratégies de migration {#sec:strategieMigration}

Il existe plusieurs manières d'effectuer la migration d'une application.
Toutes les solutions doivent respecter les contraintes définies
    Section \ref{contraintes}.

![Exemple de règle de transformation](figures/exampleRuleEngine.png){#exampleRuleEngine width=350px height=250px}

- _Migration manuelle_. Cette stratégie correspond au redéveloppement complet des applications sans l'utilisation d'outils aidant à la migration. La migration manuelle permet de facilement corriger les potentielles erreurs de l'application d'origine et de reconcevoir l'application cible en suivant les préceptes du langage cible.  
- _Utilisation d'un moteur de règles_. L'utilisation d'un moteur de règles pour migrer partiellement ou en totalité une application a déjà été appliquée sur d'autres projets [@brant2010extreme; @feldman1990fortran; @grosse2012automatic]. Pour utiliser cette stratégie, nous devons définir et créer des règles qui prennent en entrée le code source et qui produisent le code pour l'application migrée. La Figure \ref{exampleRuleEngine} montre un exemple de règle de transformation. Dans ce cas, elle permet de changer la position des opérateurs dans une expression mathématique source. L'opérateur est maintenant en suffixe de l'expression. Il est possible que la migration ne soit pas complète. Dans ce cas, les développeurs devront finir le processus de migration avec du travail manuel. L'utilisation d'un moteur de règles, bien qu'efficace, implique une solution qui n'est ni indépendante de la source ni indépendante de la cible de la migration.
- _Migration dirigée par les modèles_. La migration dirigée par les modèles implique le développement de méta-modèles pour effectuer. La stratégie respecte l'ensemble des contraintes que nous avons défini. Une migration semi-automatique ou complètement automatique est envisageable avec cette stratégie de migration. Comme pour l'utilisation d'un moteur de règle, dans le cas d'une migration semi-automatique, il peut y avoir du travail manuel à effectuer pour compléter la migration.

\newpage