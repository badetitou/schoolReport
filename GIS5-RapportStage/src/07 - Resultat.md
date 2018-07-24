# Résultats et Discussion

Nous allons maintenant présenter les résultats que nous avons obtenus suite à l'implémentation de la stratégies de migration.

Dans un premier temps, Section \ref{resultat} nous décrire les résultats de l'importation et de l'exportation.
Puis nous verrons Section \ref{visualisation} la représentation d'une application après l'avoir importée dans notre outil.
Enfin, nous discuterons des résultats Section \ref{discussion}.

## Résultats {#resultat}

Une fois l'outil implémenté, nous avons cherché à vérifier nos résultats.
Comme la stratégie mis en place est en deux étapes, _i.e._ importation et exportation,
    nous avons séparé la vérification de nos résultats en deux parties.

### Rétro-Ingénierie

\begin{table}[hbtp]
    \caption{Résultat de l'importation}
    \label{resultImport}
    \begin {center}
  %  \resizebox{\columnwidth}{!}{%
    \begin{tabular}{|c|c|c|c|}
        \hline
         Phases & Business Pages & Widgets & Link between Phases \\
        \hline
        100\% & 100\% & 98\% & 100\% \\
        \hline
    \end{tabular} %
   % }
    \end{center}
\end{table}

Le Tableau \ref{resultImport} présente les résultats que nous obtenons en exécutant la création du modèle d'interface graphique.

Pour les phases, nous avons regardé dans l'application cible le nombre de Phases existante.
Pour cela nous avons regardé dans le fichier de configuration de l'application dans lequel toutes les phases sont définis.
Puis nous avons comparé le chiffre obtenu avec le nombre de phase que nous instancions dans notre modèle.
Dans la cas d'étude avec Berger-Levrault, nous avons recensé 56 Phases, ce qui correspond exactement au nombre de phases déclaré dans le fichier de configuration.

Pour les business pages, nous avons compté le nombre classe qui implémente _IPageMetier_.
Il y en a 45.
Après l'importation, nous générons 76 pages métiers.
Nous retrouvons dans les pages métiers générées celles qui implémente l'interface _IPageMetier_ ainsi que 31 que nous avons généré.
Ces dernières proviennent de code qui a été factorisé.
Cette difficulté d'évaluation est discutée Section \ref{discussion}.

Nous réussissons à généré 2081 widgets, cependant avec les heuristiques nous avons défini Section \ref{implementationImport} nous devrions avoir 2141 widgets.
Ce qui correspond à un total de 98% de widget que nous réussissons à créer.
Il existe cependant un écart que nous n'arrivons pas encore à évaluer dont l'on discute Section \ref{discussion}.

Finalement, la detection du nombre de lien entre les phases est réussi à 100%.
Nous détectons correctement 101 liens de navigation qui existent dans l'application.
Les liens sont tous correctement liée au widget sur lequel il faut faire une action
    pour déclencher la transition et amène vers la bonne Phase.

### Exportation en Angular

Une fois l'importation complété, notre outil procède à l'exportation.
L'exportation est évaluer selon les contraintes décrites Section \ref{contraintes}.
Le code doit permettre de garder le même visuel et préserver l'architecture des éléments de l'interface.
De plus, le programme exporté doit être compilable et facile à lire pour un développeur,
    cette dernière contrainte réduit la difficulté pour les équipes de Berger-Levrault d'accepter la migration.

La lisibilité du code est évaluer selon les critères suivant :

- Nom significatif pour les variables/fonction/class
- Respect des convention du langage cible
- Indentation du code

Pour la lisibilité du code, dans le cadre de la migration l'outil conserve au maximum les noms des éléments de l'application source.
Nous supposons donc que, même si les développeurs ont mal nommé leurs variables dans l'application source, ils ne seront pas perturbé par les nouveaux noms de variable.

Le respect des conventions est n'est pas respecté à 100% actuellement.
Nous avons réussi à exporter correctement vers l'architecture Angular et la disposition des différents éléments de chaque fichier et fait correctement (par exemple, les variables sont déclarées juste après la déclaration d'une classe).
Cependant certaines convention ne sont pas respecté, notamment dans le nommage des classes que nous exportons en majuscule au lieu de respecter le pascalCase[^pascalCase].

[^pascalCase]: Pascal Case : les mots sont liés sans espace. Chaque mot commence par une Majuscule.

Pour l'indentation du code, la convention est que l'indentation correspondent à 4 espaces.
L'outil de migration exporte le code complètement indenter sauf pour les fichiers HTML.

![Home - Avant/Après](figures/cmp1.PNG){#cmp1 width=75%}

Le code exporté est compilable à 100% et est exécutable.
L'exportation conserve l'architecture entre les éléments de l'interface graphique tel que détecté dans la partie importation.
La Figure \ref{cmp1} présente les différences visuelles entre l'ancienne version, à gauche, et la nouvelle, à droite.
Nous pouvons voir que les différences sont minimes.
Dans la version exporté, les couleurs de l'en-tête des panel est un peu plus claire et l'ombre porté des panel est plus dégradé.

![Zone de saisie - Avant/Après](figures/cmp2.PNG){#cmp2 width=75%}

La Figure \ref{cmp2} présente des résultats moins bon qui sont dû à l'utilisation de layouts plus complexes.

## Visualisation {#visualisation}

![Représentation de l’application bac à sable dans sa globalité](figures/firework.png){#firework width=80%}

## Discussion {#discussion}

- Les évaluation n'ont été faîte que sur l'application _bac-à-sable_
- Il n'y a pas de gestion de certaine manière de décrire une interface en GWT (cf xml file) - mais ça ne sera pas fait
- La migration de l'application ici ne prend pas en compte les éventuelles evolutions parallèle du backend

\newpage