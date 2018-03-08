# L'Adhérence

[//]: # Adhérence
[//]: # --> pq ? quoi ? comment ?

[//]: # --> Problème
[//]: # --> Hypothèse
[//]: # --> expé-solution
[//]: # --> analyse


Si l'on souhaite migrer seulement une partie de l'application de Berger-Levrault,
  nous devons soit migrer l'ensemble de ses dépendances,
  soit les "_casser_".

Une étude de la difficulté de supprimer les liens entre deux parties d'un programme
  est donc nécessaire pour évaluer la complexité de la migration de ces dernières.
Chercher l'adhérence entre les éléments au sein des frameworks de Berger-Levrault et
  entre les frameworks utilisés par Berger-Levrault devrait me permettre d'évaluer
  la complexité à les séparer.

Mon hypothèse est que le nombre de référence entre deux éléments augmentent l'adhérence entre eux.
Je suppose que s'il y a plusieurs références entre deux classes,
  le nombre de méthode concernées par ces références est aussi un facteur augmentant l'adhérence.
L'héritage entre deux classes peut aussi représenter l'adhérence.
Le nombre de méthodes dans les classes et le nombre de méthodes redéfinies impacteraient l'adhérence.
J'ai essayé de confirmer ces hypothèses sur l'application *bac-à-sable*.

Je présente dans cette section plusieurs expériences réalisées pour tenter de
  caractériser l'adhérence entre l'application *bac-à-sable*, BLCore et GWT.
Ces expériences ont apporté quelques informations mais n'ont pas été suffisamment concluantes.
De nouvelles expériences seront nécessaires.

## Adhérences internes à un framework

Une première question est quelle est l'adhérence entre les éléments au sein de BLCore.
En effet, il s'agit de l'élément central de l'architecture écrite par Berger-Levrault.
BLCore repose sur GWT et est utilisé par les applications de Berger-Levrault.
Une solution à la migration de l'ensemble des applications serait de commencer par migrer
  BLCore, puis de migrer les applications.

Les classes essentielles de BLCore utilisées par les applications sont
  les classes définissant les widgets.
C'est donc par l'étude de l’adhérence entre ces derniers que j'ai commencé l'exploration
  du framework.

### Complexité d'un widget

![Métriques pour les widgets](figures/metricsWidget.png){#metricsWidget height=400px width=400px}

Ma première hypothèse était que l'adhérence est liée à la complexité du widget.

Un widget étant une classe, je me suis basé sur ce qui définit la complexité d'une classe.
J'ai donc calculé pour chacune des classes son nombre de lignes de code,
  sa profondeur dans la hiérarchie de classes (combien de surclasses)
  et le nombre de classes qui lui font référence.


J'ai développé un outil permettant d'extraire des informations du modèle.
Ensuite, j'ai créée un outil de visualisation de ces informations pour
  pouvoir facilement naviguer dans les résultats.
La Figure \ref{metricsWidget} présente l'outils de visualisation
  que j'ai développé avec les informations que j'ai extraites pour le widget _BLChoixListeEtOrdre_ .
Le premier panneau permet de choisir un widget pour lequel je veux détailler les informations.
Le deuxième affiche les métriques et le troisième affiche son schéma avec la hiérarchie de classes et les références.
Les nœuds verts représentent les classes faisant partie de BLCore et impliquées dans la hiérarchie de classes,
  les bleus représentent celles de GWT qui sont impliquées dans la hiérarchie de classes.
Les éléments rouges sont des classes qui sont référencées et peuvent faire partie de BLCore ou de GWT.
Un lien d'un élément vert vers un élément rouge signifie que
  la classe _"verte"_ référence la classe _"rouge"_ par un appel de méthode ou un accès à un attribut.

J'ai dans un premier temps considéré l'implémentation d'interfaces comme un héritage.

Grâce à ces informations, nous pouvons certes définir de manière plus claire la complexité d'une classe,
  mais il est encore impossible de dire si une classe est _"adhérente"_ à une autre
  et plus précisément je ne peux pas dire quelle est la classe la plus adhérente.

De plus, le schéma peut être amélioré notamment l'affichage de la hiérarchie de classe qui est présentée
  à l'inverse du sens habituel (de haut en bas plutôt que de bas en haut).
Le visuel des liens devrait également évoluer pour permettre de distinguer
  selon qu'ils désignent un héritage ou une référence entre éléments.
On devrait également changer la représentation des éléments provenant de BLCore ou GWT.
En effet, une classe provenant de BLCore peut être représenté en rouge, si elle fait partie d'une relation de référence, ou en vert, si elle fait partie de la hiérarchie de classe.

### Complexité d'un ensemble de widgets

L'impossibilité de comparer un widget à d'autres peut venir d'un manque d'informations concernant leur possibles liens.
En effet, je veux comparer des éléments pouvant être dépendants les uns des autres.
J'ai donc décidé de regrouper tous les widgets sur un même schéma.

\begin{figure}
    \centering
    \begin{minipage}{0.45\textwidth}
        \centering
        \includegraphics[width=0.9\textwidth,trim={0 0 30cm 0},clip]{figures/group/groupModif.png} % first figure itself
        \caption{\label{group} Hiérarchie de classes des widgets}
    \end{minipage}\hfill
    \begin{minipage}{0.45\textwidth}
        \centering
        \includegraphics[width=0.9\textwidth]{figures/references/referencesModif.png} % second figure itself
        \caption{\label{references} Références entre widgets}
    \end{minipage}
\end{figure}

La Figure \ref{group} présente une partie du graphe de la hiérarchie de classes définissant les widgets.
Chaque classe, et donc widget, est représentée par un rectangle.
J'ai décidé de faire varier la hauteur des rectangles en fonction du nombre de méthodes utilisées pour
  définir le widget.
De plus, la largeur change en fonction du nombre de références vers ce widget.
Cette dernière valeur peut être biaisée à cause des références provenant de l'application _bac-à-sable_.
Cependant, les deux métriques permettent d'évaluer
  la complexité d'une classe et donc d'un widget.
Nous voyons plusieurs hiérarchie de classe.
C'est ce que j'appelle des groupes.
Ces groupes mettent en évidence l'héritage entre les widgets et
  cela peut être pris en compte dans le calcul de l'adhérence entre les widgets.
La Figure \ref{group} me permet donc de dégager des groupes de widget grâce à la hiérarchie de classe.
Cela me permet de faire l'hypothèse qu'il faudrait migrer les widgets par groupe.

La Figure \ref{references} présente les références entre les classes.
J'ai représenté ces invocations entre classes grâce à un système de couches.
Les classes ne faisant appel à aucune autre se trouvent dans la première couche (en bas du schéma).
Si une classe invoque une autre classe, elle se trouvera dans la couche supérieur à celle de la
  seconde.
On retrouve également pour chaque classe, en hauteur, le nombre de références à cette dernière.
Comme pour la Figure \ref{group}, ce chiffre peut être biaisé par l'application _bac-à-sable_,
  mais nous avons néanmoins déjà un aperçu de la complexité des widgets.
La largeur ici ne varie pas.
La Figure \ref{references} m'a permis de trouver huit couches de widgets.
Grâce à cette information, j'ai émis l'hypothèse qu'il faudrait migrer les
  widgets par couches en commençant par la première car elle ne fait pas de références à une autre couche.


À partir de ces schémas, j'ai émis les hypothèses suivantes :
Il est plus simple de commencer à migrer les classes qui se trouvent en bas de la Figure \ref{references}.
Et, dans le cas de la migration d'une classe, il faudrait migrer groupe par groupe les widgets.

### Migration par couche ou groupe

![Hiérarchie des wigets](figures/groupWithRef/GroupWithRef2RogneModif.png){#groupWithRef}

Pour vérifier que la migration par couche ou groupe est plus simple,
  j'ai voulu regarder comment les groupes se positionnent par rapport aux couches et inversement.
J'ai aussi changé la manière dont les couches sont calculées de façon à ce que les classes qui s'appellent entre elles, et donc forment un cycle, soient sur la même couche.

Dans la Figure \ref{groupWithRef}, toutes les classes font partie de BLCore.
Chaque couleur correspond à un groupe.
On trouve des groupes où toutes les classes font partie de la même couche.
Ce qui montre une interdépendance entre les éléments qui proviennent des cycles que j'ai détectés.
On retrouve, par exemple, les classes *BLImage*, *BLImageViewer* et *BLViewableImage* (nommées sur la Figure \ref{groupWithRef})
  ou toutes les classes de type *BLLabel* (entourées sur la Figure \ref{groupWithRef}) qui font partie en même temps de la même couche
  et du même groupe.
Cependant, cette seule information ne permet pas de conclure sur le niveau d'adhérence entre les éléments
  car nous ne disposons d'aucune métrique sur la complexité générée par l'héritage.

![Références entre widget](figures\referencesWithGroup\referencesWithGroupModif.png){#referencesWithGroup}

La Figure \ref{referencesWithGroup} montre les références entre les widgets et met en couleurs les
  widgets provenant du même groupe.
Contrairement à la Figure \ref{references}, si il existe un cycle de dépendance entre les widgets, ils
  se trouveront dans la même couche.
De cette manière, j'élimine les références seulement liées à la hiérarchie de classe entre les widgets.
Ce schéma permet de voir si un groupe fait appel à un autre.
Cependant, il ne permet pas de définir si une référence est fortement liée à l'adhérence.
Lorsque que j'aurai défini plus précisément l'importance des liens,
  je pourrai alors confirmer ou non mes hypothèses et ainsi
  dire si la répartition par classe ou par groupe à un intérêt.

Les Figure \ref{groupWithRef} et Figure \ref{referencesWithGroup} ne m'ont pas permis de conclure
  sur l'adhérence entre les éléments d'un framework.
Des expériences doivent être menées sur l'impact d'une référence entre deux éléments sur l'adhérence entre
  ces derniers.

## Adhérence entre framework


La recherche des liens entre les classes internes à un BLCore ne m'ont pas permis
  de définir clairement ce qu'est l'adhérence ou comment l'évaluer.
J'ai donc essayé de la caractériser en analysant les liens entre deux frameworks.

### Framework vers Framework

Une première étude consiste à regarder les références entre les frameworks.
J'ai pu créer des schémas affichant les références entre l'application
  _bac-à-sable_, BLCore et GWT.

Mon objectif est de trouver quelles classes adhérent à quelles autres d'un autre framework.
Quelles sont les classes les plus adhérentes d'un framework vers un autre ?
Le but final étant de trouver les classes par lesquelles commencer la migration.

\begin{figure}
    \centering
    \begin{minipage}{0.33\textwidth}
        \centering
        \includegraphics[width=0.9\textwidth]{figures/appToCore.png} % first figure itself
        \caption{\label{appToCore} Application \textit{bac-à-sable} vers BLCore}
    \end{minipage}\hfill
    \begin{minipage}{0.33\textwidth}
        \centering
        \includegraphics[width=0.9\textwidth]{figures/coreToGWT.png} % second figure itself
        \caption{\label{coreToGWT} BLCore vers GWT}
    \end{minipage}\hfill
    \begin{minipage}{0.33\textwidth}
        \centering
        \includegraphics[width=0.9\textwidth]{figures/appToGWTModif.png} % second figure itself
        \caption{\label{appToGWT} \textit{bac-à-sable} vers GWT}
    \end{minipage}
\end{figure}

Les trois travaux que j'ai effectués m'ont permis de créer les Figure \ref{appToCore}, Figure \ref{coreToGWT} et Figure \ref{appToGWT}.

La Figure \ref{appToCore} et la Figure \ref{coreToGWT} affichent une partie du graphe
  représentant les références entre l'application _bac-à-sable_ et BLCore et entre BLCore et GWT.
Les Figures ne sont pas lisible car il y a trop d'éléments affichés.
Chaque classe a une couleur en fonction du nombre de références qu'elle reçoit ou émet.
La couleur est de plus en plus foncé si le nombre de références augmente.
Les liens possèdent aussi une largeur qui change en fonction du nombre de références
    qu'elle représente.
S'il y a beaucoup de liens entre deux classes, le trait qui le représente est plus large.
Enfin, un lien change de couleur en fonction du nombre de méthode utilisés pour faire les références.
Comme pour les classes, plus la couleur est foncée, plus le nombre de méthodes impliqué est
  important.
Le trop grand nombre d'éléments affichés sur le schéma ne permet pas de conclure sur
  l'adhérence entre les éléments.
Une visualisation plus locale, avec seulement une classe pour laquelle on regarde les références,
  devrait m'apporter plus d'information.

La Figure \ref{appToGWT} affiche les références entre l'application _bac-à-sable_ et GWT.
La hauteur des éléments représente le nombre total de références sortantes
  (pour les applications Berger-Levrault) ou entrantes (pour GWT).
Cette figure ne nous permet pas de conclure sur l'adhérence.
Elle donne néanmoins des informations sur l'application.
En effet, d'après la structure de l'application que nous avons détaillée Figure \ref{architectureBL},
  il ne devrait pas y avoir de références entre l'application _bac-à-sable_ et GWT.
Une étude de ces références mettra en évidence des différences entre ce que nous avions défini et
  la réalité du développement.

Le trop grand nombre d'informations obtenues ne permet pas de faire une visualisation
  globale simple à analyser et ne m'aide pas à définir clairement l'adhérence et comment l'utiliser pour
  comparer deux classes.

### Classe d'un framework vers les autres

La visualisation globale étant trop complexe pour apporter des résultats exploitables,
  j'ai décidé de créer des visualisations locales pour chercher
  à qualifier la complexité à migrer d'une seule classe.

![Navigateur références entre les framework](figures/glam.png){#glam width=500px height=500px}

J'ai donc créé le navigateur de la Figure \ref{glam}.
La partie de gauche me permet de sélectionner un widget parmi l'ensemble
  des widgets de BLCore.
Le panneau de droite contient deux onglets.
Ils permettent de sélectionner si l'on veut afficher les références entre
  le widget et l'application _bac-à-sable_ ou
  entre le widget et GWT.
Pour la Figure \ref{glam}, c'est l'onglet "de application vers widget" qui est sélectionné.

Grâce au navigateur je peux étudier en détail les dépendances entre la classe d'un framework et
  d'un autre framework.
Je peux donc trouver les classes les plus référencées et qualifier la complexité des références
  par leurs nombres et le nombre de méthodes impliquées dans les références des classes source et
  destination.

Cependant, je ne sais pas si le nombre de références et la qualification que j'ai faite
  sont réellement des facteurs complexifiant la migration d'une classe.
Je dois effectuer manuellement une de ces migrations d'un composant simple pour
  l'évaluer.

## Travail à faire sur l'adhérence

Toutes les informations récoltées ne m'ont pas permis de définir complètement et précisément ce qu'est l'adhérence.
Elles doivent être complétées par une évaluation de la complexité de migrer un élément de BLCore.
Ainsi je pourrai mesurer l'impact des références entre classes.

Pour cela, j'aimerais effectuer des migration manuellement.
Ces expériences me permettront de juger l'importance des références entre les classes.

\newpage
