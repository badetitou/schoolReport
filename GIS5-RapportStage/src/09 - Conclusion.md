# Conclusion

Durant ce stage, j'ai continué le projet de migration que j'ai initié pendant
    mon projet de fin d'étude à Polytech Lille.
Après une première étape d'analyse que j'avais mené durant ce stage,
    j'ai approfondi les recherches de l'état de l'art et créer des outils permettant de faciliter la migration des applications de Berger-Levrault.

La première étape de mon stage a été la définition formel des éléments composant une application d'une interface graphique.
Une application graphique est divisée en trois parties, l'interface graphique, le code comportemental et le code métier.
Ensuite, j'ai défini les différentes contraintes à respecter pour répondre aux besoins de Berger-Levrault
    et conçu un processus permettant d'effectuer la migration en suivant ces obligations.
Puis, j'ai implémenté en partie les outils nécessaire pour effectuer la migration
    en suivant le processus de migration.
Ainsi, j'ai pû commencer l'exportation de certains éléments des applications GWT en Angular.

La stratégie de migration est bien défini et que l'implémentation du modèle d'interface graphique avec les outils d'importation et d'exportation donnent des résultats encourageant sur la suite du projet.
Il reste encore du travail d'implémentation et de définition de méta-modèles.
En effet, je n'ai pas encore conçu les méta-modèles pour les _layout_, le code comportementale et le code métier.
Une fois conçus, il faudra les implémenter tout en gardant la structure du projet coherent.

Un autre travail doit être mené sur la validation des résultats.
En effet, même si pour les cas simples que nous avons étudié il est possible de vérifier
    la validé de l'exportation _"à la main"_.
Une fois l'exportation complètement implémenté, il faudra développé des outils permettant d'évaluer la complétude de mon travail.