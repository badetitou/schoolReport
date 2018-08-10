# Résultats et Discussion

Nous allons maintenant présenter les résultats que nous avons obtenus suite à l'implémentation de la stratégie de migration.

Dans un premier temps, Section \ref{resultat} nous décrivons les résultats de l'importation et de l'exportation.
Puis nous verrons Section \ref{visualisation} la représentation d'une application après l'avoir importée dans notre outil.
Enfin, nous discuterons des résultats Section \ref{discussion}.

## Résultats {#resultat}

Une fois le prototype implémenté, nous avons cherché à vérifier nos résultats.
Comme la stratégie mise en place est en deux étapes, importation et exportation,
    nous avons séparé la vérification de nos résultats en deux parties.

### Rétro-Ingénierie {#retroInge}

\begin{table}[hbtp]
    \begin {center}
  %  \resizebox{\columnwidth}{!}{%
    \begin{tabular}{|c|c|c|c|}
        \hline
         Phases & Business Pages & Widgets & Link between Phases \\
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
Cette difficulté d'évaluation est discutée Section \ref{discussion}.

Nous réussissons à identifier 2081 widgets, cependant avec les heuristiques que nous avons définis Section \ref{implementationImport} nous devrions avoir 2141 widgets.
Ce qui correspond à un total de 98 % de widget que nous réussissons à créer.
Il existe cependant un écart que nous n'arrivons pas encore à évaluer dont l'on discute Section \ref{discussion}.

Finalement, la détection du nombre de liens entre les phases est réussie à 100 %.
Nous détectons correctement 101 liens de navigation qui existent dans l'application.
Les liens sont tous correctement connectés aux widgets sur lesquels il faut faire une action
    pour déclencher la transition et amène vers la bonne Phase.

### Exportation en Angular {#exportToAngular}

Une fois l'importation complétée, notre outil procède à l'exportation.
L'exportation est évaluée selon les contraintes décrites Section \ref{contraintes}.
Le code doit permettre de garder le même visuel et préserver l'architecture des éléments de l'interface.
De plus, le programme exporté doit être compilable et facile à lire pour un développeur,
    cette dernière contrainte réduit la difficulté pour les équipes de Berger-Levrault d'accepter la migration.

La lisibilité du code est évaluée selon les critères suivants :

- Nom significatif pour les variables, les fonctions et les classes
- Respect des conventions du langage cible
- Indentation du code

Pour la lisibilité du code, dans le cadre de la migration, l'outil conserve au maximum les noms des éléments de l'application source.
Nous supposons donc que, même si les développeurs ont mal nommé leurs variables dans l'application source, ils ne seront pas perturbés par les nouveaux noms de variable.

Le respect des conventions n'est pas respecté à 100 % actuellement.
Nous avons réussi à exporter correctement vers l'architecture Angular et la disposition des différents éléments de chaque fichier est faite correctement (par exemple, les variables sont déclarées juste après la déclaration d'une classe).
Cependant certaines conventions ne sont pas respectées, notamment dans le nommage des classes que nous exportons en majuscule au lieu de respecter le pascalCase[^pascalCase]
    qui est la norme Angular.

[^pascalCase]: Pascal Case : les mots sont liés sans espace. Chaque mot commence par une Majuscule.

Pour l'indentation du code, la convention est que l'indentation équivaut à 4 espaces.
L'outil de migration exporte le code complètement indenté sauf pour les fichiers HTML.

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
La migration a bien permis de conserver l'architecture entre les éléments, ce que l'on peut voir avec les composants de formulaire à l'intérieur du panel _"zone de saisie"_.
Cependant le layout n'a pas été correctement respecté, ce qui explique les différences visuelles entre les deux images.

## Visualisation {#visualisation}

Pendant la construction de l'outil de migration, nous avons créé des requêtes sur le modèle d'interface graphique.
Ces requêtes permettent de créer des graphiques et d'analyser la construction de l'interface graphique sans regarder le code source.

![Extrait de la représentation de l’application _bac à sable_](figures/firework.png){#firework width=80%}

La Figure \ref{firework} présente un extrait de la visualisation des relations entre les
    différentes entités de l'interface graphique de l'application _bac à sable_.
Les phases sont représentées en noires, les widgets en vert et les business page en rouge.
Une phase contenant une business page, une business page en contenant une autre ou un widget et un widget en contenant un autre sont représentés par une flèche partant du conteneur vers le contenu.
Les liens de navigation sont représentés par une flèche bleue partant du widget sur lequel il faut effectuer une action vers la Phase qui est appelée.

On peut voir sur la Figure \ref{firework} des agglomérats de widget.
Ceux-ci représentent des widgets container comme des panels, qui contiennent d'autres widgets, comme des boutons ou du texte.

## Discussion {#discussion}

Les résultats que nous avons obtenus peuvent être discutés.
En effet, ils peuvent être remis en cause en partie pour les raisons suivantes.

Bien que nous ayons testé régulièrement notre travail sur les applications de production de Berger-Levrault,
    la recherche des patterns pour l'importation ainsi que l'évaluation de la migration n'a été faîte que sur l'application
    _bac à sable_.
Nous savons que l'application après migration compile, mais nous n'avons pas de retours sur la réussite de l'exportation du visuel.
Il est aussi possible que les autres logiciels de Berger-Levrault contiennent des déviances dans le code que nous n'avons pas prévu
    ce qui peut nuire au résultat final.

\begin{figure}[htb]
\centering
\begin{lstlisting}
<bl_grille id="panelCoche" largeur="100%" remplissage="5" espace="1" hauteur="300px">
  <bl_ligne>
    <bl_cellule alignementhorizontal="centre" alignementvertical="milieu">
      <bl_cadre_epais titre="Boutons radio" largeur="100%">
         <bl_bouton_radio id="radio1" groupe="groupe 1" libelle="option 1"/>
         <bl_bouton_radio id="radio2" groupe="groupe 1" libelle="option 2"/>
         <bl_bouton_radio id="radio3" groupe="groupe 2" libelle="option 3"/>
         <bl_bouton_radio id="radio4" groupe="groupe 2" libelle="option 4"/>
       </bl_cadre_epais>
    </bl_cellule>
  </bl_ligne>
</bl_grille>
\end{lstlisting}
\caption{Définition d'interface graphique via fichier XML}
\label{xml}
\end{figure}

En GWT, il est possible de définir une interface graphique grâce à un fichier XML.
La Figure \ref{xml} présente un extrait d'un fichier de l'application _bac à sable_ qui permet de générer une interface graphique.
La ligne déclare un panel de type grille.
Il contient une _"ligne"_ déclarée ligne 2 et 4 _"radio button"_ déclarés ligne 5 à 8.
Dans le cadre de ce projet, seule l'application _bac à sable_ utilise cette technique pour définir des interfaces.
Nous avons donc décidé de ne pas traiter l'importation des widgets pour les business pages définit de cette manière.
En les considérant, le pourcentage de widget que nous arrivons à importer se voit réduit.
