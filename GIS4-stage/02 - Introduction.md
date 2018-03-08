# Introduction

## Contexte

Afin de terminer ma quatrième année d'école d'ingénieurs à Polytech Lille, j'ai effectué un stage à l'Inria Lille - Nord Europe au sein
  de l'équipe RMOD.
L'équipe RMOD travaille sur le développement du langage de programmation Pharo (smalltalk) et la remodularisation.
Mon stage a duré 4 mois et a été supervisé par Madame ETIEN et Monsieur BLONDEAU.

## Objectifs

Le développement applicatif est une pratique complexe et les développeurs doivent fournir des
  logiciels fiables rapidement.
Les logiciels doivent aussi être maintenables.
C'est pourquoi des outils ont été développés pour les langages de programmation facilitant ces aspects.
Un de ces outils est le Test Unitaire.
Ce sont des petits programmes permettant de tester une ou plusieurs fonctionnalités d'un programme complexe.
En Pharo, langage que j'ai utilisé pendant mon stage, plus de 42&nbsp;000 tests sont lancés quotidiennement sur Jenkins[^Jenkins].
Nous pourrions ajouter à ce nombre les tests écrits par les développeurs pour leurs projets.

Lancer les tests liés à un projet est nécessaire pour s'assurer de son bon fonctionnement.
Mais les lancer peut être une manipulation coûtant plusieurs minutes pour de petites modifications de code.
Mon objectif principal a donc été dans un premier temps d'étudier comment les tests sont utilisés par les développeurs utilisant Pharo afin, dans un seconds temps,
 de créer une solution leur permettant de gagner du temps pour leurs développements,
  c'est-à-dire sélectionner et lancer les tests nécessaires après une modification du code source par les développeurs.


[^Jenkins]: Jenkins est un serveur d'automatisation open-source. Il peut être utilisé pour automatiser tout type de tâche comme la construction, le test, et le déploiement d'une application.

\newpage
