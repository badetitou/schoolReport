# Résultats

## Recherche bibliographique

La première étape de mon travail a été de le positionner par rapport à la littérature existante.

Il existe trois sous-grands domaines qui auraient pu correspondre.

- Library Migration est le domaine qui parle des changements de librairie dans un langage. Dans mon cas, cela va être un problème lorsque l'on va vouloir retranscrire un comportement défini par une librairie JAVA vers une librairie Angular.
Cependant la majorité des articles de ce sous-domaine discute surtout de la migration de librairie d'un langage vers le même langage.

- Monolithic into Microservices discute de la division de sous-ensembles d'applications pour en créer des plus petites.
Comme pour la Library Migration, la bibliographie reste majoritairement dans le même langage

- Widget Migration semble être la partie de la litérature qui correspond le mieux à ce que je fais.
On discute de la détection des widgets et de comment réécrire ceux-ci dans un autre langage.
En particulier, on y retrouve un article de @samir2007swing2script dans lequel on parle de la migration de Swing vers ajax.
Cependant il utilise une analyse dynamique de l'application pour détecter les comportements.
Au vu de la taille des applications de Berger-Levrault ainsi que de l'impact de l'utilisation d'une telle stratégie sur les utilisateurs, je ne pourrai pas utiliser la même stratégie.
Je pourrai peut être réutiliser le XUL décrit brièvement dans ce document pour représenter le comportement des widgets de mon travail.


## Outils d'analyse

### Décomposition du projet

![Étapes de mon travail](figures/Resume BL - Page 1.png){#etapes width=300 height=300}


Grâce à la recherche bibliographique, j'ai décidé des différentes grandes étapes que je vais devoir mener pour mon travail.

Une première étape est d'extraire du code source d'une application un modèle abstrait des différents widgets de l'application.

Ensuite, je dois convertir ces différents widgets vers un modèle abstrait qui contient pour chacun le visuel et le comportement du widget en fonction des actions qu'on lui applique.

Enfin, déveloper un parseur prenant en entrée le modèle abstrait en ressortant le nouveau code source.


### Décomposition d'une application

Après analyse et discussion avec Berger-Levrault, j'ai pu extraire la structure générale de leurs applications.

![Structure Application Berger-Levrault](figures/Structure application Berger-Levrault.png){#structure width=300 height=300}

Comme le présente la Figure \ref{structure}, tous les "widgets" provenant de GWT ont été sous-classés par un "widget" Berger-Levrault (ex: BLTextField).

L'application utilise deux fichiers de configuration.

- *application.module.xml* détaille l'ensemble des phases (fenêtres) de l'application.
- *coreincubator.gwt.xml* permet de définir les scripts et les css à utiliser dans l'application. Le fichier décrit aussi des redéfinitions de classes qui se font au moment de la compilation.
Ci-dessous, la classe BLUserPreferencesNull sera remplacée à la compilation par BLUserPreferences. Berger-Levrault utilise ce type de fichier pour définir le comportement de ses logiciels à compilation et non via des options sélectionnables par l'utilisateur.

```xml
<replace-with class="fr.bl.client.core.refui.base.gwt.BLUserPreferencesNull">
      <when-type-is class="fr.bl.client.core.refui.base.gwt.BLUserPreferences" />
</replace-with>
```

On précise que Berger-Levrault n'utilise pas cette technique pour changer lors de la compilation le visuel de ses widgets, mais cela peut changer leurs comportements.

#### Précision sur les Phases

Une phase contient une ou plusieurs pages métiers.
Si elle contient plusieurs pages métiers, celles-ci seront représentées par des sous-onglets dans l'application web.

C'est `Workspace.getPhaseManager()`{.java} qui permet de gérer l'ouverture des phases et la commande `ConstantsPhase.Util.get().NOM_DE_LA_PHASe()`{.java} permet d'ouvrir une nouvelle phase. Cependant, il faut regarder dans le fichier *application.module.xml* de l'application pour récuperer les vrais liens.

Ex:

```java
vpErgo.add(new BLLinkLabel("Internationalisation",
   ConstantsPhase.Util.get().PHASE_MAQUETTE_I18N(),
   null, AbstractDesk.TYPE_TAB_UNIQUE));
```

En cliquant sur le BLLinkLabel, on ouvrira la phase PHASE_MAQUETTE_I18N.
Dans le fichier xml, on recherche donc PHASE_MAQUETTE_I18N et on trouve

```xml
<phase codePhase="PHASE_MAQUETTE_I18N"
  codeValue="GEN_I18N" className="fr.bl.client.coremaquette.PhaseMaquetteI18n"
    raccourci="I18" titre="I18n" description="Exemple d'intertionalisation." />
```

C'est donc la classe fr.bl.client.coremaquette.PhaseMaquetteI18n qui sera réellement cherchée.

Les phases peuvent être de deux types :

- unique : l'onglet est ouvert une fois, puis actualisé en fonction des demandes
- multiple : un onglet est ouvert à chaque demande

### Construction du modèle abstrait
![Modèle d'une application de Berger-Levrault V1](figures/Modele BL App - Modele.png){#model width=300 height=300}

Il a ensuite été possible de créer un modèle d'une application de Berger-Levrault que je vais pouvoir exploiter.

La Figure \ref{model} permet de représenter une application de Berger-Levrault.
On y retrouve les Phases et les Pages métiers ainsi que les widgets et leurs actions.
C'est ce modèle que j'essaie de visualiser dans mon analyse.

La Figure \ref{correspondance} présente les modèles faisant les liens entre les classes du modèle que j'ai créé et le modèle JAVA.

### Extraction du modèle

Pour faire mon analyse, j'ai utilisé l'outil Moose^[[Moose est une plateforme pour l'analyse de logiciels et données.](http://www.moosetechnology.org/)].
Pour le moment je travaille sur la première étape, l'extraction de l'architecture d'une application de Berger-Levrault.
L'objectif est de réussir à séparer les différents widgets.

![Correspondance Modèle - JAVA](figures/Modele BL App - Correspondance.png){#correspondance width=500 height=350}

La première étape est la génération d'un fichier _.mse_.
Pour cela j'ai utilisé l'outil VervaineJ.
Ce logiciel me permet de créer le fichier _.mse_ depuis le code source d'une application (sans avoir besoin de le compiler).
J'avais avant cela essayé d'utiliser l'outil jdt2famix.
Mais cet outil a besoin du code source compilé de l'application.
Cependant les applications de Berger-Levraut sont composées de plusieurs petits projets.
Cette structure de projet rend l'utilisation de jdt2famix impossible pour le moment.

### Analyse des résultats

La page d'accueil type d'une application de Berger-Levrault est présentée Figure \ref{bacASable}.
Comme nous pouvons le voir, il y a beaucoup de liens qui existent depuis cette phase vers d'autres.
Ce sont tous les liens présents sur la gauche de la page Web.
Cette page web est générée avec de nombreux Widgets _BLLinkLabel_ comme on peut le voir dans la code Figure \ref{code}.
Les Widgets sont donc naturellement recodés en des liens HyperTextes.


\begin{figure}
    \centering
    \begin{minipage}{0.45\textwidth}
        \centering
        \includegraphics[width=0.9\textwidth]{ScreenShot/HubApp.png} % first figure itself
        \caption{\label{bacASable} Page d'accueil bac à sable}
    \end{minipage}\hfill
    \begin{minipage}{0.45\textwidth}
        \centering
        \includegraphics[width=0.9\textwidth]{ScreenShot/Code.png} % second figure itself
        \caption{\label{code} Page d'accueil code JAVA}
    \end{minipage}
\end{figure}

J'ai travaillé sur le modèle Moose pour rechercher grâce à une analyse statique du code les différents éléments représentés dans le modèle Figure \ref{model}.
En particulier, j'ai cherché à retrouver une phase avec de nombreux liens vers d'autres (comme j'ai trouvé juste avant).

![Relation entre les phases](figures/lightvisu2.png){#visu2 width=300 height=300}

La Figure \ref{visu2} est une sortie que j'ai réussi à obtenir représentant l'application de Berger-Levrault.
Les cercles bleus représentent les différentes phases.
Les cercles rouges représentent les pages métiers.
Les arcs, la possibilité d'aller d'une phase à une autre grâce à un widget s'ils vont d'une phase à une autre.
Si un arc va d'une phase à une page métier, c'est l'utilisation d'une page métier par une phase qui est représentée.

On repère  aisement deux HUB (phases qui ont des liens vers beaucoup d'autres phases).
En utilisant Moose et le code source de l'application, j'en ai déduit que c'était les _home_ que nous avons détectés pendant l'analyse du code source.

Nous pouvons aussi voir des phases seules, ce sont soit des phases de tests qui n'ont donc réellement aucun lien avec le reste de l'application, soit des phases utilitaires qui sont utilisées dans des phases abstraites que je ne peux pas actuellement exploiter avec Moose car il me manque des sources de l'application.

\newpage
