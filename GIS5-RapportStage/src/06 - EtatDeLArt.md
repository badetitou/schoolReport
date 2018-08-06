# Etat de l'art

Dans le cadre de la conception de l'outil de migration,
    nous avons étudié la littérature pour identifier
    les techniques respectant les critères définit par Berger-Levrault.
Dans un premier temps, la Section \ref{migrationTechnique} présente les différentes
    techniques qui sont utilisés pour faire effectuer la migration d'application.
Dans un second temps, la Section \ref{positionnement} positionnera notre travail
    par rapport à ceux proposé dans la littérature et utilisant la même technique
    de migration.

## Technique de migration {#migrationTechnique}

Nous avons extrait de la littérature plusieurs domaine de recherche connexe au
    travail que nous voulons mener sur la migration d'application.
Les approchent proposées permettent soit d'effectuer la migration d'application
    sans répondre aux critères que nous avons définit avec Berger-Levrault, soit
    de proposer des pistes à explorer pour créer une solution.

### Rétro-Ingénierie d'interface graphique

La rétro-ingénierie discute de comment représenter une interface graphique
    dans l'objectif de pouvoir l'analyser et comment générer cette représentation.
Le choix de représentation des interfaces graphiques est discuté Section \ref{positionnement}.

Les auteurs utilisent trois techniques pour instancier leurs méta-modèles.

**Statique**. La stratégie statique consiste à analyser le code source de l'application comme on analyserait du texte. en fonction du language source, l'analyse statique peut être plus ou moins facile à mettre en place.

Dans le cas de [@cloutier2016wavi], les auteurs ont pû analyser directement les fichier HTML, CSS et javascript.
Ceci leurs permet de construire un arbre syntaxique du code source du site web
    et d'extraire les différents widgets depuis le fichier html.

D'autre auteurs comme [@silva2010guisurfer;@lelli2016automatic;@staiger2007reverse] utilisent un outil qui va analyser du code source provenant de langage non destiné au web mais permettant de décrire une interface graphique.
Leurs cas d'études sont des applications de bureau ayant une interface graphique.
Ces logiciels vont chercher la définissions des widgets dans le code source.
Une fois les créations des widgets trouvés dans le code source, le logiciel va analyser méthodes invoquées ou invoquant les widgets afin de découvrir les relations entre les widgets et leurs attributs.

Sánchez Ramón *et al.* [@sanchez2014model] ont développé une solution permettant d'extraire depuis un ancien logiciel son interface graphique.
Le cas d'étude des auteurs est une application source ayant était créée avec Oracle Forms.
Contrairement aux cas d'études précédant, l'interface est définit dans un fichier à part explicitant pour chaque widget sa position fixe.
Chaque widget a ainsi une position X, Y ainsi qu'une hauteur et une largeur.
La stratégie de l'auteur consiste à determiner la hiérarchie des widgets depuis ces informations.

**Dynamique**. La stratégie statique consiste à analyser l'interface graphique
    d'une application pendant qu'elle est en fonctionnement.
L'utilisateur va lancer un logiciel dont l'interface est à extraire,
    puis il va lancer l'outil d'analyse dynamique.
Ce dernier va être capable de détecter les différents composants de l'interface et les actions qu'il peut leurs appliquer.
Puis l'outil va appliquer une action sur un élément de l'interface est détecter les changements s'il y en a.
En répétant ces actions, la stratégie va permettre d'explorer les différentes interfaces qui sont utilisées dans l'application à analyser et comment interagir avec ces dernières.

Memon *et al.* [@MemonWCRE2003] ont développé un logiciel nommé "GUI Ripper".
Cet outil est une implémentation de la stratégie dynamique qui s'applique sur des applications exécutées sur un ordinateur (application de bureau).
Une adaptation serait nécessaire pour coller au spécificité des applications web comme dans notre cas.

[@samir2007swing2script;@shah2011reverse;@morgado2011reverse]

**Mixe**.

### Transformation de modèle vers modèle

### Transformation de modèle vers texte

### Migration de librairie

### Migration de langage

## Positionnement sur la migration via les modèles {#positionnement}

\newpage