# Description du problème {#sec:descProbleme}

Dans le contexte de l'évolution des applications de Berger-Levrault,
    l'entreprise a estimé la transformation du code à 4000 jours-hommes de développement.
Ceci s'explique majoritairement par les 1,67 MLOC[^mloc] utilisés pour les logiciels.
Un de mes objectifs à Berger-Levrault est de définir une stratégie de migration
    qui réduise le temps nécessaire pour la transformation des programmes.

[^mloc]: MLOC : _Million Lines of Code_

## Contraintes {#sec:contraintes}

Berger-Levrault étant une importante entreprise dans le domaine de l'édition de logiciel,
    elle a des contraintes spécifiques vis-à-vis d'un outil de migration.
En effet, la solution logicielle doit respecter les contraintes suivantes :

- _Depuis GWT (BLCore) et vers Angular_. Dans le cas d'une automatisation ou semi-automatisation du processus de migration,
    celui-ci doit pouvoir prendre du code GWT en entrée et s'achever par la génération de code Angular.
    La solution peut contenir des structures facilitant son utilisation pour d'autres langages cibles mais pas au détriment du projet fixé avec Berger-Levrault.
- _Approche modulaire_. Une approche divisée en petites étapes améliorerait la maintenabilité de l'outil de migration [@sanchez2014model].
    Elle permettrait de facilement remplacer une étape ou de l'étendre sans introduire d'instabilité.
    Le respect de cette contrainte offre plus de contrôle sur le processus de migration aux les entreprises.
    L'approche modulaire permet, entre autres, aux entreprises de modifier l'implémentation de la stratégie pour respecter leurs contraintes spécifiques.
- _Préservation de la structure_. Après la migration, nous devons retrouver la même structure entre les différents composants de l'interface graphique (_c.-à-d._ un bouton qui appartenait à un panel dans l'application source appartiendra au panel correspondant dans l'application cible).
    Cette contrainte permet de faciliter le travail de compréhension de l'application générée par les développeurs.
    En effet, ils vont retrouver la même structure qu'ils avaient dans l'application source.
- _Préservation du visuel_. La migration doit pouvoir conserver le visuel aussi proche que possible.
    Cette contrainte est particulièrement importante pour les logiciels commerciaux.
    En effet, les utilisateurs de l'application ne doivent pas être perturbés par la migration.
    Il est aussi possible que Berger-Levrault ait envie de profiter de la migration
        pour rafraîchir le visuel de leurs applications.
    Dans ce cas le l'outil peut proposer de faciliter certains points de cette transformation graphique.
- _Automatique_. Une solution automatique facilite l'accessibilité de l'outil.
    Pour simplifier le processus de migration, il serait bien que les utilisateurs de l'outil n'aient pas à intervenir pendant le processus de migration ou très peu.
    Ainsi, l'outil peut être utilisé avec un minimum de connaissance préalable.
    Dans le cas où le prototype n'a pas besoin de l'intervention humaine pendant le processus de migration,
        il sera plus facile à utiliser sur de grand système [@moore1994knowledge].
- _Amélioration de la qualité_. La migration doit permettre de traiter le maximum de déviance du programme source possible.
    Il est possible que certaines déviances ne soient pas traitables ou demande un effort trop important,
        elles seront alors traité post-migration.
    Dans le cas de Berger-Levrault, l'outil de migration peut gérer les éléments, utilisés par l'application à migrer, provenant du _framework_ GWT.
- _Continuation du service_. Pendant la conception de la stratégie de migration, le développement du prototype permettant la migration et la migration elle-même,
        les équipes de développement doivent pour continuer la maintenance des applications.
    Cette contrainte est essentielle puisque Berger-Levrault, en raison de son activité, ne peut pas demander à ses clients d'accepter
        un arrêt des améliorations et correction de bug pendant plusieurs mois.
- _Lisibilité_. Afin de facilité le travail de compréhension du code migré,
        il serait bien que la migration produise une application respectant les normes définies par les développeurs.
    Dans le cas de Berger-Levrault, il s'agit du respect du nommage des variables en CamelCase[^CamelCase]
        et l'utilisation de nom significatif.

[^CamelCase]: _Camel Case_ : les mots sont liés sans espace. Chaque mot commence par une Majuscule.

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

Dans le cas de ce projet, les langages de programmations source et cible ont deux architectures différentes.
Les différences sont syntaxiques, sémantiques et architecturales.
Pour la migration d'application GWT vers Angular, les fichiers _.java_ seront décomposé en plusieurs fichiers Angular.

Le Tableau \ref{comparaison} synthétise les différences entre l'architecture d'une application en Java et celle en Angular.
Les différences se font pour trois notions, les pages web, leurs styles et les fichiers de configuration.

Avec le _framework_ GWT, un seul fichier Java est nécessaire pour représenter une page web.
L'ensemble de la page web peut donc être contenu dans ce fichier,
    même s'il reste possible de décomposer les différents éléments de la page web en plusieurs classes.
Les fichiers Java contiennent les différents composants graphiques (widgets) de la page web, leurs positions les uns par rapport aux autres et leurs organisations hiérarchiques.
Dans le cas d'un widget sur lequel une action peut être exécutée (comme un bouton), c'est dans ce
    même fichier qu'est contenu le code à exécuter lorsque l'action est réalisée.

En Angular, on crée une hiérarchie de fichier correspondant à un sous-projet pour chaque page web.
Cette hiérarchie permet de séparer le visuel d'une page web, des scripts qu'il utilise.
Elle contient plusieurs fichiers dont un fichier HTML qui contient les widgets de la page web et leurs organisations,
    et un fichier TypeScript contenant le code à exécuter quand une action se produit sur un widget.
On a donc la décomposition d'un fichier Java pour GWT en deux fichiers HTML et TypeScript en Angular pour représenter les widgets et le code qui leur est associé.

Pour le style visuel d'une page web, dans le cas de GWT, il y a un fichier CSS commun à toutes les pages web et des modifications qui sont appliquées directement dans le fichier Java de la page web.
Ces modifications peuvent porter sur la couleur ou les dimensions.

En Angular, on retrouve le même fichier CSS général pour tout le projet.
Il est aussi possible de définir un fichier CSS pour chaque sous-projet (page web) qui va définir le visuel des éléments de la page web.
Il y a donc création d'un fichier supplémentaire en Angular par rapport à GWT.

Pour les fichiers de configurations, GWT n'a besoin que d'un fichier de configuration qui définit
    les fichiers java correspondant à une page web et les URL que l'on doit utiliser pour y accéder.
En Angular, il y a deux fichiers de configuration générale. Le premier, _module_, explicite les différentes pages web accessibles dans l'application ainsi que les services distants et les composants graphiques utilisables dans l'application. Le second, de _routing_, définit pour les différentes pages web de l'application leurs chemins d'accès.
