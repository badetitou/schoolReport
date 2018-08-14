# Résultats {#sec:resultat}

Nous allons maintenant présenter les résultats que nous avons obtenus suite à l'implémentation de la stratégie de migration.

Nous avons expérimenté notre approche sur l'application _bac à sable_ de Berger-Levrault.
Nous présentons dans cette Section les résultats que nous obtenons aux différentes étapes du processus de migration.
\secref{retroImport} présente les résultat de la phase de rétro-ingénierie.
Puis, nous présentons, \secref{visualisation}, une visualisation que nous avons créé pour analyser
    le modèle que nous avons instancié.
Finalement, \secref{exportToAngular}, nous comparons le résultat final avec les contraintes que nous avons fixé \secref{contraintes}.

## Résultat de l'importation {#sec:retroImport}

\begin{table}[hbtp]
    \begin {center}
  %  \resizebox{\columnwidth}{!}{%
    \begin{tabular}{|c|c|c|c|}
        \hline
         Phases & Business Pages & Widgets & Link between Phases \\
        \hline
        56 & 76 & 2081 & 101 \\
        \hline
        100 \% & 100 \% & 98 \% & 100 \% \\
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
Nous retrouvons dans les pages métiers celles qui implémentent l'interface _IPageMetier_ ainsi que 31 qui
    dernières proviennent de code qui a été factorisé par les développeurs de l'application.
La factorisation du code est une complication dans le calcul du nombre exact de business page que nous devons détecter pendant l'importation.
Cette difficulté d'évaluation est discutée \secref{discussion}.

Nous réussissons à identifier 2081 widgets, cependant avec les heuristiques que nous avons définis \secref{implementationImport} nous devrions avoir 2141 widgets.
Ce qui correspond à un total de 98 % de widget que nous réussissons à créer.
Il existe cependant un écart que nous n'arrivons pas encore à évaluer dont l'on discute \secref{discussion}.

Finalement, la détection du nombre de liens entre les phases est réussie à 100 %.
Nous détectons correctement 101 liens de navigation qui existent dans l'application.
Les liens sont tous correctement connectés aux widgets sur lesquels il faut faire une action
    pour déclencher la transition et amène vers la bonne Phase.

## Visualisation {#sec:visualisation}

Pendant la construction de l'outil de migration, nous avons créé des requêtes sur le modèle d'interface graphique.
Ces requêtes permettent de créer des graphiques et d'analyser la construction de l'interface graphique sans regarder le code source.

![Extrait de la représentation de l’application _bac à sable_](figures/largeFireworkModif.png){#fig:largeFirework width=70%}

La \figref{largeFirework} présente un extrait de la visualisation des relations entre les
    différentes entités de l'interface graphique de l'application _bac à sable_.

Le rond noir est la représentation d'une phase.
Ici, il correspond à la phase _"Sample\_Editable\_List"_.
Nous pouvons retrouvé comment la phase est appelé en suivant la flèche bleu.
La phase contient aussi une business page, représenté en rouge,.
Il s'agit de la page _"SamplePageEditableLists"_.
Cette dernière contient 9 widgets, représentés en vert.
Celui du dessous est la représentation d'une instance d'un composant BLGrid provenant de BLCore.
Ce type de composant peut contenir d'autre widgets.
Dans cette exemple il en contient deux.

Nous pouvons voir sur le côté gauche de l'image un agglomérats de widget.
Cette disposition d'élément est courante,
    elle représente un widget de type _container_ qui contient beaucoup d'autre widgets pouvant être des _leafs_ ou des _containers_.
Ici, il s'agit d'un BLGrid qui contient des textes et des champs de saisies.

La représentation complète que nous obtenons est disponible en Annexe.

## Exportation en Angular {#sec:exportToAngular}

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
\caption{Phase Home} \label{cmp1}
\end{figure}

Le code exporté est compilable à 100 % et est exécutable.
L'exportation conserve l'architecture entre les éléments de l'interface graphique tels que détectés dans la partie importation.
La Figure \ref{cmp1} présente les différences visuelles entre l'ancienne version, à gauche, et la nouvelle, à droite.
Nous pouvons voir que les différences sont minimes.
Dans la version exportée, les couleurs de l'en-tête des panels sont un peu plus claires et l'ombre portée des panels est plus dégradée.

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
\caption{Phase Zone de saisie} \label{cmp2}
\end{figure}

La Figure \ref{cmp2} présente les différences visuelles pour la Phase _Zone de saisie_ de l'application _bac àsable_.
L'image de gauche correspond à la phase avant la migration tandis que celle de gauche est la même Phase après la migration.
Les deux images étant grandes, nous les avons rognés pour afficher cette zone d'intérêt.
Bien que les deux images ont l'air complètement différentes,
    tous les widgets sont présent dans la version migré.
La différence visuel est du a un problème dans la gestion des layout.
Ce point est discuté \secref{discussionLayout}.
