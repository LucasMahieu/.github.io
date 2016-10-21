---
title: Notes de Cours
sidebar: mydoc_sidebar
permalink: cours_mtsoc.html
folder: mydoc/mtsoc
---

#### Lien du cours sur [Github](https://github.com/moy/cours-tlm)

# Cours 10/10/2016

Il faut créer/modéliser une hiérarchie des composants pour respecter le fonctionnement logique : video en même temps que audio et non séquenciel

## Les Communications

Pour les communications :
	- Un maitre : initie la com
	- Un esclave : répond à un requête   
Différent protocoles : 
	- Bloquants : initie un échange et ATTEND la réponse
	- Non bloquant : ne bloque pas l'initiateur + cannaux de com requête + réponse  

## Les interuptions

Connections direct entre des composants par 1 fils  
* Permet de ne pas faire de polling
* Permet de faire des économie de l'énergie 

Pas standardisé en TLM => on fait ce qu'on veut 

__Question ?__  
Comment faire communiquer 2 procs ensemble ?  
Il faut une mémoire et faire du polling si les proc sont initiateur seulement.


## Apport du TLM 
* Développement du code embarqué en avance de phase
* Débuggage de l'intégration des composants
* Validation du RTL

## C++

Voir slide sur C++ du prof [sur son Github](https://github.com/moy/cours-tlm/blob/master/02-c-plus-plus-handout.pdf)

# Cours du 21/10/2016

## System C
Voir slide d'exemple de code [sur son Github](https://github.com/moy/cours-tlm/blob/master/03-systemc-handout.pdf)

__Comment ça marche à l'intérieur ?__

System C à un scheduleur intern qui gère les processus : il a une "event list" et la liste des processus.  
3 états dans le scheduleur :   
- Running
- Sleeping
- Éligible

Pour sortir de running c'est forcement que le processus à "rendu la main" en faisant un sleep.  
Un proc qui est running ne peut que devenir Sleeping.  

## Revision (encore) de C++
Voir les slides [sur le github](PAS ENCORE LA) du prof.



{% include links.html %}
