# Résultats {#sec:resultat}

Nous allons maintenant présenter les résultats que nous avons obtenus suite à l'implémentation de la stratégie de migration.

Nous avons expérimenté notre approche sur l'application _bac à sable_ de Berger-Levrault.
Nous présentons dans cette Section les résultats que nous obtenons aux différentes étapes du processus de migration.
\secref{retroImport} présente les résultats de la phase de rétro-ingénierie.
Puis, nous présentons, \secref{visualisation}, une visualisation que nous avons créé pour analyser
    le modèle que nous avons instancié.
Finalement, \secref{respectContraintes}, nous comparons le résultat final avec les contraintes que nous avons fixé \secref{contraintes}.

## Résultat de l'importation {#sec:retroImport}

\begin{table}[hbtp]
    \begin {center}
  %  \resizebox{\columnwidth}{!}{%
    \begin{tabular}{ccccc}
        \hline
         & \textbf{Phases} & \textbf{Business Pages} & \textbf{Widgets} & \textbf{Link between Phases} \\
        \hline
        \textbf{Nombre d'éléments} & 56 & 76 & 2081 & 101 \\
        \textbf{Pourcentage bien migré} & 100 \% & 100 \% & 93 \% & 100 \% \\
        \hline
    \end{tabular} %
   % }
    \end{center}
    \caption{Résultat de l'importation}
    \label{resultImport}
\end{table}

Le Tableau \ref{resultImport} présente les résultats que nous obtenons en exécutant la création du modèle d'interface graphique.

Pour les phases, nous avons regardé dans l'application cible le nombre de phases existantes.
Pour cela nous avons regardé dans le fichier de configuration de l'application dans lequel toutes les phases sont définies.
Puis nous avons comparé le chiffre obtenu avec le nombre de phases que nous instancions dans notre modèle.
Dans le cas d'étude avec Berger-Levrault, nous avons recensé 56 phases, ce qui correspond exactement au nombre de phases déclaré dans le fichier de configuration.

Pour les business pages, nous avons compté 45 classes qui implémentent _IPageMetier_.
Après l'importation, nous identifions 76 pages métiers.
Nous retrouvons dans les pages métiers celles qui implémentent l'interface _IPageMetier_ ainsi que
    31 qui proviennent de code qui a été factorisé par les développeurs de l'application.
La factorisation du code est une complication dans le calcul du nombre exact de business page que nous devons détecter pendant l'importation.
Cette difficulté d'évaluation est discutée \secref{discussion}.

Dans la littérature, nous n'avons pas trouvé de manière pour évaluer l'exportation de widgets de manière automatique.
Nous avons donc décidé de prendre un échantillon de phases parmi toutes celles présentes dans l'application _bac à sable_.
L'échantillon est composé de 6 phases de taille différente et qui comporte différents types de widgets,
    ce qui correspond à environ 12% du nombre total de phases.
Pour chaque phase de l'échantillon,
    nous avons comparé dans le code originel la structure de la page web, qu'elle représente en analysant
    les instanciations de widgets et les appels aux méthodes permettant de définir la structure de la page,
    avec celle obtenue dans la phase migrée.
Nous avons relevé un total de 93 % de widget que nous réussissons à migrer.

Finalement, la détection du nombre de liens entre les phases est réussie à 100 %.
Nous détectons correctement 101 liens de navigation qui existent dans l'application.
Les liens sont tous correctement connectés aux widgets sur lesquels il faut faire une action
    pour déclencher la transition et aux Phase sur lesquels l'action amène l'utilisateur.

## Visualisation {#sec:visualisation}

Pendant la construction de l'outil de migration, nous avons créé des requêtes sur le modèle d'interface graphique.
Ces requêtes permettent de créer des graphiques et d'analyser la construction de l'interface graphique sans regarder le code source.

![Extrait de la représentation de l’application _bac à sable_](figures/largeFireworkModif.png){#fig:largeFirework width=70%}

La \figref{largeFirework} présente un extrait de la visualisation des relations entre les
    différentes entités de l'interface graphique de l'application _bac à sable_.

Le rond noir est la représentation d'une phase.
Ici, il correspond à la phase _"Sample\_Editable\_List"_.
Nous pouvons retrouver comment la phase est appelée en suivant la flèche bleu.
La phase contient aussi une business page, représenté en rouge.
Il s'agit de la page _"SamplePageEditableLists"_.
Cette dernière contient 9 widgets, représentés en vert.
Celui du dessous est la représentation d'une instance d'un composant BLGrid provenant de BLCore.
Ce type de composant peut contenir d'autres widgets.
Dans cet exemple il en contient deux.

Nous pouvons voir sur le côté gauche de l'image un agglomérat de widget.
Cette disposition d'élément est courante,
    elle représente un widget de type _container_ qui contient beaucoup d'autres widgets pouvant être des _leafs_ ou des _containers_.
Ici, il s'agit d'un BLGrid qui contient des textes et des champs de saisies.

La représentation complète que nous obtenons est disponible en Annexe.

## Respect des contraintes {#sec:respectContraintes}

Le prototype que nous avons conçu devait respecter les contraintes suivantes,
    fonctionne depuis GWT vers Angular,
    utilise une approche modulaire,
    préserve la structure de l'application,
    préserve le visuel de l'application,
    est automatique ou demande peu d'intervention de l'utilisateur,
    améliore la qualité du logiciel,
    permet la continuation de service
    et génère du code lisible.

Tout d'abord, le prototype permet bien de traiter du code Java en entrée et
    génère du code Angular.
Nous précisons que le code généré est compilable et peut être exécuté.
On peut donc visualisé les résultats en lançant le logiciel migré.

L'implémentation que nous avons réalisée est facilement portable pour être utilisable avec
    une autre technologie source ou une autre technologie cible.
De même, il est facile d'ajouter un nouveau type de widget à exporter en Angular,
    ou à ajouter à l'analyse statique, sans introduire d'instabilité.
Cependant, nous n'avons pas prévu de solution pour facilement modifier une étape
    de l'importation ou de l'exportation.
Par exemple, modifier la manière dont les widgets sont détectés demande à l'utilisateur du prototype
    de comprendre le fonctionnement complet de la partie importation.

Nous avons évalué la préservation de la structure en analysant la visualisation que nous
    avons conçue et présentée \secref{visualisation}.
Le prototype détecte et conserve l'architecure pendant l'importation et
    l'exportation la restitue correctement.
Il n'y a que les interfaces générées avec un fichier XML que nous avons décidé de ne pas gérer.
Ce dernier point est discuté \secref{interfaceXML}.

\begin{figure}
\begin{subfigure}{0.45\textwidth}
\includegraphics[width=\linewidth,trim={0 10cm 30cm 0},clip]{figures/cmp1/avant.png}
\caption{Avant} \label{fig:cmp1a}
\end{subfigure}
\hspace*{\fill} % separation between the subfigures
\begin{subfigure}{0.45\textwidth}
\includegraphics[width=\linewidth,trim={0 10cm 30cm 0},clip]{figures/cmp1/apres.png}
\caption{Après} \label{fig:cmp1b}
\end{subfigure}
\caption{Phase Home} \label{fig:cmp1}
\end{figure}

La \figref{cmp1} présente les différences visuelles entre l'ancienne version, à gauche, et la nouvelle, à droite.
Nous pouvons voir que les différences sont minimes.
Dans la version exportée, les couleurs de l'en-tête des panels sont un peu plus claires, l'ombre portée des panels est plus dégradée
    et les lignes sont un peu plus espacées

\begin{figure}
\begin{subfigure}{0.45\textwidth}
\includegraphics[width=\linewidth,trim={16cm 10cm 15cm 0},clip]{figures/cmp2/avant.png}
\caption{Avant} \label{fig:cmp2a}
\end{subfigure}
\hspace*{\fill} % separation between the subfigures
\begin{subfigure}{0.45\textwidth}
\includegraphics[width=\linewidth,trim={0cm 12cm 30cm 1cm},clip]{figures/cmp2/apres.png}
\caption{Après} \label{fig:cmp2b}
\end{subfigure}
\caption{Phase Zone de saisie} \label{fig:cmp2}
\end{figure}

La \figref{cmp2} présente les différences visuelles pour la Phase _Zone de saisie_ de l'application _bac à sable_.
L'image de gauche correspond à la phase avant la migration tandis que celle de gauche est la même Phase après la migration.
Les deux images étant grandes, nous les avons rognés pour afficher cette zone d'intérêt.
Bien que les deux images ont l'air complètement différentes,
    tous les widgets sont présents dans la version migré.
La différence visuelle est due à un problème dans la gestion des layout.
Ce point est discuté \secref{discussionLayout}.

Notre outil est automatique.
En effet, une fois le prototype développé,
    l'importation et l'exportation n'ont pas besoin de l'intervention d'un utilisateur.

Grâce à la modularité que nous avons introduite dans notre application,
    il est possible d'améliorer la qualité du logiciel originel.
Dans le cas des applications de Berger-Levrault utilisant BLCore, notre prototype peut être utilisé pour enlever les mauvaises utilisations
    du _framework_ GWT depuis une application.

Les premières étapes de développement de l'outil de migration non pas produit de discontinuité du service.
Le développement des applications des Berger-Levrault a continué sans être impacté.
Cependant, si à la fin du développement du prototype nous n'arrivons pas à migrer 100 % de l'application,
    il y a aura une discontinuité du service pendant que les développeurs finissent manuellement la migration.

Enfin, nous avons voulu produire du code lisible pour les développeurs.
Pour cela, en plus de respecter les conventions courantes du développement logiciel,
    le prototype conserve le nom des variables originel choisi par les développeurs.
Cela permet d'améliorer la lisibilité et favorise la compréhension de l'application migré.
