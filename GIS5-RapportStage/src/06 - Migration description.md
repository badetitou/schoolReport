# Mise en place de la migration par les modèles {#sec:secImplementation}

Suite à l'étude des contraintes inhérentes aux problèmes de migration dans le cadre d'une entreprise,
    et à l'état de l'art que nous avons mené,
    nous avons travaillé sur la conception et l'implémentation d'une stratégie de migration
    respectant les critères que nous avons fixés.

\secref{approcheMigration}, nous présentons les différentes approches que nous avons identifié pour effectuer la migration.
Puis, \secref{processusMigration} nous décrivons le processus de migration que nous avons conçu.
Et enfin \secref{metamodelUI} et \secref{metamodelComportemental} nous présentons les
    différents outils que nous avons du créer ainsi que leurs implémentations.

## Approches pour effectuer la migration {#sec:approcheMigration}

Nous avons identifié plusieurs manières d'effectuer la migration d'une application.
Toutes les solutions doivent respecter les contraintes définies \secref{contraintes}.

- _Migration manuelle_. Cette stratégie correspond au redéveloppement complet des applications sans l'utilisation d'outils aidant à la migration.
    La migration manuelle permet de facilement corriger les potentielles erreurs de l'application d'origine et de reconcevoir l'application cible en suivant les préceptes du langage cible.  
- _Utilisation d'un moteur de règles_. L'utilisation d'un moteur de règles pour migrer partiellement
        ou en totalité une application a déjà été appliquée sur d'autres projets [@brant2010extreme; @feldman1990fortran; @grosse2012automatic].
    Pour utiliser cette stratégie, nous devons définir et créer des règles qui prennent en entrée le code source et qui produisent le code pour l'application migrée.
    par exemple, une règle de transformation permettant de déplacer l'opérateur d'une expression mathématique
        en la plaçant en tant que suffice de l'expression transformerai l'expression `"4 + 2"` en `"4 2 +"`.
    Il est envisageable d'effectuer la migration partiellement avec cette approche.
    Dans ce cas, les développeurs devront finir le processus de migration avec du travail manuel.
    L'utilisation d'un moteur de règles, bien qu'efficace, implique une solution qui n'est ni indépendante de la source ni indépendante de la cible de la migration.
- _Migration dirigée par les modèles_. La migration dirigée par les modèles implique le développement de méta-modèles.
    La stratégie respecte l'ensemble des contraintes que nous avons défini.
    Une migration semi-automatique ou complètement automatique est envisageable avec cette stratégie de migration.
    Comme pour l'utilisation d'un moteur de règle, dans le cas d'une migration semi-automatique, il peut y avoir du travail manuel à effectuer pour achever la migration.

L'utilisation d'un moteur de règles comme la migration dirigée par les modèles sont envisageable.
Cependant, la migration par les modèles pourrait permettre de généralisé nos prototypes
    et d'après notre étude de la littérature c'est une approche courante pour effectuer la migration d'application.
Nous avons donc décidé d'utiliser cette approche pour effectuer la migration.

## Processus de migration {#sec:processusMigration}

![Schéma processus de migration](figures/processusMigration.png){#fig:processusMigration width=100%}

À partir de l'état de l'art et des contraintes que nous avons explicitées,
    nous avons conçu une stratégie pour effectuer la migration.
Le processus que l'on a représenté \figref{processusMigration} est divisé en cinq étapes :

1. _Extraction du modèle de la technologie source_ est la première étape permettant de construire l'ensemble des analyses et transformations que nous devons appliquer pour effectuer la migration. Elle consiste en la génération d'un modèle représentant le code source de l'application originel. Dans notre cas d'étude, le programme source est en Java et donc le modèle que nous créons est une implémentation d'un méta-modèle permettant de représenter une application écrite en Java.
2. _Extraction de l'interface graphique_ est l'analyse du modèle de la technologie source pour détecter les éléments qui relèvent du modèle d'interface graphique. Ce dernier, que nous avons dû concevoir, est expliqué \secref{metamodelUI}.
3. _Extraction du code comportemental_. Une fois le modèle d'UI généré, il est possible d'extraire le code comportemental du modèle de la technologie source et de créer les correspondances entre les éléments faisant partie à la fois du code comportemental et du modèle d'interface graphique (UI). Par exemple, si un clic sur un bouton agit sur un texte dans l'interface graphique. L'extraction du code comportemental permet de définir que pour le bouton, défini dans le modèle UI, lorsqu'un clic est effectué, on effectue un certain nombre d'actions, dont une sur le texte, lui aussi défini dans le modèle UI.
4. _Exportation de l'interface graphique_. Le modèle d'interface graphique étant construit et les liens entre interfaces utilisateur et code comportemental créés, il est possible d'effectuer l'exportation de l'interface graphique. Cela consiste en la génération du code du langage source exprimant uniquement l'interface graphique. C'est aussi à cette étape que l'on génère l'architecture des fichiers nécessaires au fonctionnement de l'application cible ainsi que la création des fichiers de configuration inhérente à l'interface.
5. Finalement, l'_Exportation du code comportemental_ est la génération du code comportemental qui est lié à l'interface graphique. Cette étape peut être effectuée en parallèle de la précédente.

## Méta-modèle d'interface graphique {#sec:metamodelUI}

![Méta-Modèle d'interface graphique](figures/GUIModel.png){#fig:guiModel width=80%}

Afin de représenter les interfaces utilisateurs des applications de Berger-Levrault,
    nous avons conçu le méta-modèle illustré \figref{guiModel}.
Ce méta-modèle est la synthèse des méta-modèles que nous avons trouvé lors de l'état de l'art et présenté \secref{stateMetaUI}.
Dans la suite de cette section, nous présentons les différentes entités du méta-modèle.

La **Phase** représente le conteneur principal d'une page interface graphique.
Cela peut correspondre à une _fenêtre_ d'une application du bureau, une page web, ou
    dans notre cas d'un onglet à une page web.
Dans le modèle KDM, une Phase correspond à un _Screen_ (type de UIDisplay).

Une Phase peut contenir plusieurs **Business Page**.
Elle peut aussi être appelée par un widget grâce à une **Action** de type **Call Phase**.
Lorsqu'une Phase est appelée, l'interface change pour l'afficher.
Dans le cas d'une application de bureau, l'interface change ou une nouvelle fenêtre est ouverte avec l'interface de la Phase.
Pour une application web, l'appel d'une Phase peut correspondre à l'ouverture d'un nouvel onglet, le changement d'onglet actif ou la transformation de la page web courante.
Cette notion de business page n'est présente dans aucun des autres papiers,
    elle est inhérente au projet de Berger-Levrault et peux facilement être modifier pour correspondre à ce que l'on
    trouve dans la littérature.
Il suffirai de changer la relation entre phase et business page pour une relation entre phase et widget.

Les **Widgets** sont les différents composants d'interface et les composants de disposition.
Il existe deux types de widgets.
Le **Leaf** est un widget qui ne contient pas d'autre widget.
Le **Container** qui peut contenir un ou plusieurs autres widgets.
Ce dernier permet de séparer les widgets en fonction de leur place dans l'organisation de la page web représentée,
    pour cela nous utilisons le patron de conception _composite_ fortement utilisé dans la littérature.

Les **Attributes** représentent les informations appartenant à un widget et peuvent changer son aspect visuel ou son comportement.
Des attributs communs sont la hauteur et la largeur pour définir précisément la dimension d'un widget.
Il y a aussi des attributs qui contiennent des données.
Par exemple, un widget représentant un bouton peut avoir un attribut _text_ qui explicite le texte du bouton.
Un attribut peut changer le comportement d'un widget, c'est le cas de l'attribut _enable_.
Un bouton avec l'attribut _enable_ positionné sur _false_ représente un bouton sur lequel nous ne pouvons pas cliquer.
Enfin, les widgets peuvent avoir un attribut qui aura un impact sur le visuel de l'application.
Ce type d'attribut permet de définir un layout à respecter par les widgets contenus dans un autre
    et potentiellement les dimensions de ce dernier pour respecter une mise en pages particulière.
Nous ne retrouvons pas la notion d'attribut dans les méta-modèles proposé par l'OMG,
    cependant ils ont été introduit par les papiers [@gotti2016java;@sanchez2014model;@morgado2011reverse;@garces2017white;@memon2007eventflow;@samir2007swing2script;@shah2011reverse;@joorabchi2012reverse;@MemonWCRE2003;@mesbah2012crawling;@amalfitano2012using;@silva2010guisurfer].

Les __Actions__ sont propres aux widgets.
Elles représentent des actions qui peuvent être exécutées dans une interface graphique.
C'est une notion qui est présente dans les méta-modèles proposé par l'OMG.
__Call Service__ représente un appel à un service distant comme une URL sur internet.
__Fire PopUp__ est l'action qui affiche un pop-up sur l'écran.
Le pop-up ne peut pas être considéré comme un widget,
    il n'est pas présent dans l'interface graphique,
    il apparaît seulement et disparaît.

Le __Service__ est la référence de fonctionnalité distante que l'application peut appeler à partir de son interface graphique.
Dans le contexte d'une application client/serveur, il peut s'agir du côté serveur de l'application.

## Implémentation du processus

Pour tester la stratégie, nous avons implémenté un outil qui suit le processus de migration.
L'outil a été implémenté en Pharo[^pharo] et nous avons utilisé la plateforme Moose[^moose].

![Implémentation de l'outil](figures/codeImpl.png){#codeImpl width=350px height=250px}

Le Figure \ref{codeImpl} présente la logique d'implémentation.
Le bloc principal est _BL-Model_.
Ce bloc contient l'implémentation du méta-modèle GUI[^GUI].
En plus du modèle, il y a un exportateur abstrait et une implémentation de
    l'exportateur pour Angular (_BL-Model-Exporter_ et _BL-Model-Exporter-Angular_)
    et un importateur abstrait ainsi que le code spécifique pour Java (_BL-Model-Importateur_ et _BL-Model-Importer-Java_).
Parce que nous testons notre solution sur le système de Berger-Levrault,
    nous avons également implémenté l'extension _"CoreWeb"_,
    la stratégie de migration ne dépend cependant pas de cette extension.
Ces paquets étendent les précédents pour avoir un contrôle fin du processus de migration.
Ce contrôle est important pour améliorer le résultat final.

[^moose]: Moose est une plateforme pour l'analyse de logiciels et de données - [http://www.moosetechnology.org/](http://www.moosetechnology.org/)
[^pharo]: Pharo est un langage de programmation objet, réflexif et dynamiquement typé - [http://pharo.org/](http://pharo.org/)
[^GUI]: GUI : Graphical User Interface - Interface Graphique

### Importation {#sec:implementationImport}

La création des modèles représentant l'interface graphique est divisée en trois étapes comme présenté Section \ref{sec:processusMigration}.
Dans le cas de Berger-Levrault, nous avons implémenté l'approche en Pharo avec Moose.

La première étape est la conception du modèle de la technologie source.
Ce modèle avait déjà une implémentation existante dans Moose avec le projet _Famix-Java_.
Nous avons donc réutilisé ce modèle pour ne pas avoir à reconcevoir un modèle préexistant.
De plus, ce travail préliminaire est compatible avec plusieurs outils qui ont été développés
    en interne à RMod.
Entre autres, deux logiciels de génération du modèle Famix-Java depuis du code source Java existent.
Les outils sont verveineJ[^verveineJ] et jdt2Famix[^jdt2famix].
Ces deux derniers permettent de créer depuis le code source un fichier _mse_.
Le fichier _mse_ peut ensuite être importé dans la plateforme Moose.
Dans le cadre du projet avec Berger-Levrault, nous avons utilisé verveineJ car ce dernier permet aussi de _garder un lien_ entre le modèle
    généré et le code à partir duquel il l'a été.

Une fois le modèle de la technologie source créé, et après avoir implémenté nos méta-modèles,
    nous avons développé des outils en Pharo permettant d'effectuer la transformation du modèle source vers le modèle GUI.
Nous allons maintenant décrire les techniques utilisées pour retrouver les éléments définis dans le modèle GUI depuis le modèle de technologie source.

\begin{figure}[htb]
\centering
\begin{lstlisting}
public class SamplePageMetier1 extends AbstractSimplePageMetier {
    @Override
    public void buildPageUi(Object object) {
        BLLinkLabel lblPgSuivante = new BLLinkLabel("Next page");
        lblPgSuivante.addClickHandler(new ClickHandler(){
            @Override
            public void onClick(ClickEvent event) {
                SamplePageMetier1.this.fireOnSuccess("param 1");
            }
        });
        lblPgSuivante.setEnabled(false);
        // add the content
        vpMain.add(new Label("<Business content>"));
        vpMain.add(lblPgSuivante);
        super.setBuild(true);
    }
}
\end{lstlisting}
\caption{Définition d'interface graphique en Java}
\label{fig:uiJava}
\end{figure}

La \figref{uiJava} présente un extrait du code de l'application _bac à sable_.
Il s'agit de la méthode `buildPageUi(Object object)` qui contient le code à exécuter pour construire l'interface graphique de
    la business page "SamplePageMetier1".

Les premiers éléments que nous avons voulu reconnaître sont les phases.
En analysant les projets GWT, nous avons repéré un fichier _.xml_ dans lequel est stocké toutes les informations des phases.
Nous avons donc ajouté une étape à l'importation qui est l'analyse d'un fichier _XML_.
Ce fichier nous permet de _"facilement"_ récupéré la classe java correspondant à une phase,
    ainsi que le nom de la phase.

Ensuite, nous avons développé l'outil d'importation de manière incrémentale.
Nous avons donc cherché les business pages.
Grâce à l'analyse préliminaire des applications de Berger-Levrault, nous avons détecté que
    les business pages en GWT correspondent à des classes qui implémente l'interface _IPageMetier_.
Dans la \figref{uiJava}, la classe SamplePageMetier1 étend AbstractSimplePageMetier, et ce dernier
    implémente l'interface.
Une fois les classes trouvées, nous avons recherché les appels des constructeurs des classes.
Puis, en faisant le lien entre les appels et les phases qui _"ajoute"_ à leurs contenus,
    nous avons détecté les liens d'appartenances entre les business pages et les phases.

Pour les widgets, nous avons dû tout d'abord trouver tous les widgets potentiellement instanciable.
Pour cela, nous avons cherché toutes les sous-classes Java de la classe GWT _Widget_.
Ce sont les classes qui vont pouvoir être instanciées et utilisées pour la construction du programme.
Ensuite, comme pour les business pages, nous avons cherché les appels des constructeurs des widgets et
    avons relié ces appels à la business page qui les a ajoutés.
Dans la \figref{uiJava}, il y a deux appels à des constructeurs de widget.
Le constructeur de BLLinkLabel est appelé ligne 4 et celui de Label ligne 13.
La variable `vpMain` correspond au panel principal de la business page.
Les lignes 13 et 14 correspondent à l'ajout d'un widget dans une business page
    grâce aux appels à la méthode `add()`.

Enfin, pour la détection des attributs et des actions associés à un widget.
Nous avons, pour chaque widget, cherché dans quelle variable Java il a été affecté.
Puis nous avons cherché les appels de méthodes effectués depuis ces variables java.
Les appels aux méthodes _"addActionHandler"_ sont transformés en action tandis que
    les appels aux méthodes _"setX"_ ont été transformés en attribut.
Cette approche pour détecter les attributs et les actions provient des papiers de la littérature [@silva2010guisurfer;@samir2007swing2script].
Dans le cas de la \figref{uiJava},
    le BLLinkLable, dont la variable est `lblPgSuivante`, est lié à une action et à un attribut.
Les lignes 5 à 10 correspondent à l'ajout d'une action et le code à executer quand l'action est exécutée.
La ligne 11 correspond à l'ajout de l'attribut "Enabled" avec comme valeur `false`.

[^verveineJ]: [verveineJ : https://rmod.inria.fr/web/software/](https://rmod.inria.fr/web/software/)
[^jdt2famix]: [jdt2famix : https://github.com/feenkcom/jdt2famix](https://github.com/feenkcom/jdt2famix)

### Exportation

Une fois la génération du modèle d'interface graphique et du modèle du code comportemental terminé,
    il est possible de lancer l'exportation.
L'exportation consiste en la génération du code de l'application cible.

La première étape de l'implémentation de l'exportation est l'utilisation
    du patron de conception _"visiteur"_.
Ce dernier est appliqué au modèle d'interface graphique.

La visite du modèle GUI crée la hiérarchie de l'application cible ainsi que les fichiers de configuration.
Ensuite, l'exportation visite toutes les phases.
Pour chacune des phases, considérées comme des sous-projets en Angular dans l'architecture de l'application cible que nous avons définie,
    le visiteur génère les fichiers de configurations.
Puis, pour chaque business page, le visiteur génére un fichier HTML et un fichier TypeScript.
Pour le fichier html, le visiteur construit le DOM à partir des widgets contenus dans la business page.
Les widgets connaissant leurs attributs et actions,
    ils fournissent eux-mêmes leurs caractéristiques aux visiteurs.
Ces caractéristiques englobent la génération du code comportemental.
