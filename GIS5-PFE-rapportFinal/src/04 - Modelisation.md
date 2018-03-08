# Modélisation

La création des outils d'analyses est la tâche qui m'a pris le plus de temps.
Elle est divisée en deux parties.
Une première partie consiste à décrire une application de Berger-Levrault.
La seconde partie correspond à la création des visualisations
  à partir de la structure d'une application.

Berger-Levrault m'a fourni leur application _bac-à-sable_
  afin de pouvoir tester mon travail sur une application à taille humaine et
  normalement construite suivant les mêmes principes que les applications en production.

## Structure d'une application

Après analyse et discussion avec Berger-Levrault, j’ai pu extraire la structure générale de
leur application _bac-à-sable_.

L'application _bac-à-sable_ utilise les fichiers de configuration suivants :

- *application.module.xml* qui détaille l'ensemble des phases (pages web) de l'application.
- *coreincubator.gwt.xml* qui permet de définir les scripts et les css à utiliser dans l'application. Le fichier décrit aussi des redéfinitions de classes qui se font au moment du preprocessing.


Ci-dessous, la classe *BLUserPreferencesNull* sera remplacée durant le preprocessing par *BLUserPreferences*. Berger-Levrault utilise ce type de fichier pour définir le comportement de ses logiciels à la compilation et non via des options sélectionnables par l'utilisateur.

```xml
<replace-with class="fr.bl.client.core.refui.base.gwt.BLUserPreferencesNull">
      <when-type-is class="fr.bl.client.core.refui.base.gwt.BLUserPreferences" />
</replace-with>
```

![Page web de l'application bac-à-sable de Berger-Levrault](figures/screenshot/HubAppModif2.png){#website width=500px height=500px}

La Figure \ref{website} présente une page web de l'application _bac-à-sable_.
J'en détaille la structure ci-dessous et
  je fais correspondre les éléments de l'application avec la Figure \ref{website}.

Les applications de Berger-Levrault sont composées de **Phases** qui correspondent aux pages web auxquelles nous pouvons accéder.
Dans la Figure \ref{website}, la phase correspond à la partie encadrée en noir.
Les phases sont représentées par une classe Java et sont identifiables grâce au fichier *application.module.xml* qui les nomme.
Le code ci-dessous correspond à la déclaration de la phase *PHASE_MAQUETTE_I18N*
  qui est liée à la classe *fr.bl.client.coremaquette.PhaseMaquetteI18n*.

```xml
<phase codePhase="PHASE_MAQUETTE_I18N"
  codeValue="GEN_I18N" className="fr.bl.client.coremaquette.PhaseMaquetteI18n"
    raccourci="I18" titre="I18n" description="Exemple d'intertionalisation." />
```

Une phase ne contient pas l'ensemble du code correspondant à une page web,
  mais contient des **Pages Métiers**.
Les pages métiers sont identifiables dans le programme via les classes qui implémentent *IPageMetier*.
Ce sont les pages métiers qui vont décrire une portion d'une page web.
Dans la Figure \ref{website}, il n'y a qu'une page métier.
Elle est encadrée en rouge.

Certaines phases sont dites _stand-alone_.
Cela signifie qu'elles ne contiennent pas de page métier et décrivent seules
  la totalité des pages web auxquelles elles correspondent.

Les phases stand-alone et pages métiers peuvent contenir des instances de **Widget**.
Il s'agit des composants graphiques que l'on peut voir sur la page web (_i.e._ les boutons, les tableaux, les listes, etc.)
  et des objets qui définissent le placement des composants sur une interface graphique (_i.e._ les panels).
J'ai mis en évidence, en vert, certains widgets dans la Figure \ref{website} (un tableau, un label, des liens).
Tous les éléments graphiques de la page web présenté Figure \ref{website} sont des widgets.
Les widgets sont définis dans _BLCore_.


![Meta-Model d'un application de Berger-Levrault](figures/Modele BL App - Modele.png){#metaModel width=500px height=500px}

Ces derniers peuvent avoir des **Actions**.
A ce jour, j'ai travaillé sur deux types d'actions :

- les **actions d'appels** qui correspondent au changement d'une page.
  Par exemple le clic sur un lien hypertexte.
- les **actions asynchrones** qui font une requête sur un **Service** distant.
  Par exemple une liste qui se rempli après le chargement du site.

Je n'ai pas encore pris en compte les actions qui visent à effectuer des changements sur la
  page web courante.
Par exemple, l'utilisateur est sur une page web qui contient un formulaire,
  et en fonction des choix qu'ils effectuent (comme cocher ou non une boite à cocher) des éléments graphiques
  doivent s'afficher.
Ce sont les actions dites **actions complexes**.

À partir de toutes ces informations, j'ai conçu un meta-modèle de l'application de Berger-Levrault.
Comme on peut le voir Figure \ref{metaModel}, on y retrouve les différents éléments composant une l'application.



## Outils d'analyse

Une fois les différents concepts présents dans l'application et les liens existants entre eux identifiés, j'ai cherché à décrire les applications.

Pour ce faire, j'ai extrait les différentes éléments de l'application à partir du code source de l'application grâce à l'outil de modélisation Moose[^moose] (implémenté en Pharo[^pharo]).

L'instanciation des concepts identifiés à la Figure \ref{metaModel}, pour l'application *bac-à-sable* s'est faite par requêtage du code source. Par exemple une classe implémentant l'interface *IPageMetier* est une page métier.

J'ai trouvé un total de 56 phases et 57 pages métiers.
Il y a aussi 4356 instanciations de widget au sein de l'application.

J'ai également étudié la complétude de mon travail.
Toutes les phases et toutes les pages métiers sont détectées.
Toutefois, il reste encore quelques problèmes dans la détection des instances de widgets et
  dans les liens entre ces derniers et les services ou phases à cause d'une association
  absente dans le meta-modèle que j'ai construit.

## Visualisation

![Représentation de l'application bac à sable dans sa globalité](figures/feuDartifice/feuArtificeModif2.png){#feuArtifice  width=700px height=600px}

La Figure \ref{feuArtifice} présente la visualisation du modèle de l'application _bac-à-sable_.
On y retrouve deux importantes composantes connexes et de nombreuses petites.

Les deux composantes devraient être reliées pour n'en former qu'une.
Je n'ai pas encore réussi à extraire certaines informations du code source
  ce qui m’empêche d'ajouter les liens entre les composantes.

On voit dans la composante située en partie haute deux hubs (entourés en noir sur la Figure \ref{feuArtifice}).
En analysant le code source de l'application,
  j'ai retrouvé ces éléments.
Ce sont les _home_ de l'application, respectivement la `PhaseMaquetteHome` et `PhaseAccueilIncubator`.

Les liens gris représentent l'appartenance d'un élément à un autre
  (_i.e._ un lien gris allant d'une page métier à un widget signifie que le widget est contenu dans la page métier).
Les _fleurs_ de la Figure \ref{feuArtifice} sont donc la représentation de pages web.
Elles contiennent la phase au centre et ses éventuelles pages métiers ainsi que les widgets qui les composent autour.

Les liens bleus représentent le passage à une autre page métier en utilisant un widget.
Les fleurs sont donc logiquement reliées les unes aux autres par des liens bleu.
Ce sont les actions d'appels.

Les éléments oranges représentent des services potentiellement appelés.
On en retrouve beaucoup qui n'ont pas de lien vers l'application.
J'ai vérifié et ils ne sont effectivement pas appelés.

Certaines fleurs ne sont reliées à des deux importantes composantes connexes.
Ce sont des fleurs composées de classes utilitaires.
Elles permettent de faire des tests.

Cette visualisation a été construite de façon itérative.
Elle permet de mettre en évidence certaines erreurs,
  par exemple dans cette version il manque certains éléments dans le meta-modèle ou
  il y a une erreur dans la façon de construire le modèle de l'application _bac-à-sable_.
En effet, la présence des deux importantes composantes connexes distinctes ne reflète pas la réalité.
Il manque un lien entre les deux.
Reste à trouver lequel et comment le mettre en évidence.
C'est par observation des différentes visualisations obtenues que nous avons complété
  le meta-modèle initialement défini avec l'aide de Berger-Levrault, à partir de la connaissance des développeurs.

## Travail futur

Une fois que toutes les composantes d'une application seront bien détectées et ajoutées au modèle,
  je pourrai faire une étude de scalabilité de cette solution sur des applications réelles de Berger-Levrault.
Cela me permettra aussi de vérifier que le modèle que j'ai créé est valable pour toutes les applications
  de Berger-Levrault et que les visualisations que j'ai créées sont exploitables sur les applications en production.

Il y aura donc une étape de recherche sur les outils et visualisations que l'on peut construire au-dessus du modèle.

[^moose]: [Moose est une plateforme pour l'analyse de logiciels et données.](http://www.moosetechnology.org/)
[^pharo]: [Pharo est un langage de programmation objet, réflexif et dynamiquement typé ](http://pharo.org/)

\newpage
