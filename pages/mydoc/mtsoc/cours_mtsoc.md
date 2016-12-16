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

# Cours 16/12/16 

## Emballage natif 

Attention au boucle infinie dans laquelle le processus ne rend pas la main.  
Dans ce cas, la simu ne s'arrêtera pas ...  
Il faut faire appelle à cpu_relax pour rendre la main.  

Inconvénients de l'emballage natif:

- Pas de support de l'asm
- Pas de visibilité des autres transactions (accès pile, tas, intructions
  (fetch)).
- Analyse de performances difficile

## Simulateur de jeu d'instructions

exemple : ISS Instruction Set Simulator 

Émule de jeu d'instruction 

Plusieurs niveaux de précision : 
- Instruction accurate
- Cycle accurate
- Cycle callable 

Mais l'émulation a le principale inconvénients que c'est lent  

## Autre solution : 

- Just-in-Time (JIT) : Compiler le code, et découper le binaire en bloc de base
  et jongler entre l'ISS et les blocs de bases recompiler 
- Ou autre translations dynamique : QMU etc 

## Si il y a un OS ?

Avec ISS : ca marche mais trop lent 

Solution native :   
On pourrait porter Linux sur une architecture "System C TLM"  


## Évaluation et exploration d'architecture

Comment est géré le temps en système C ?  
wait(temps) -> change l'ordre des actions, vrai notion de temps  

Rôle du temps en TLM ?

Plusieurs niveau de vue du temps : 

- TLM Loosely Time (LT): temps moue, le temps à pas de signification, gros grain.
  (utilisé : programmation)
- Approximately Timed (AT) : Temps prècis induits par la microarchi,
  communication à la taille du bus. (utilisé pour l'évaluation d'archi et de
perf)

Problématiques : 

- Exemple de lecture écriture en mémoire :   
En LT -> |      READ      |     WRITE     |
En AT -> |READ|WRITE|READ|WRITE|READ|WRITE|

en LT, tout pourrait être fait dans le même temps ! 

- Transducteur : C'est des convertisseurs qui permet de gérer le temps

# Bug or not Bug ?  

Il peut y avoir des bugs HW et SW:

- HW : La simulation simule EXACTEMENT le HW qui avait un BUG, donc on peut corriger
  le HW -> cool
- HW : Erreur de programmation du simulateur
- SW : Bug de soft -> on débug 
- SW : Bug du soft à cause du HW -> aie aie aie 

Si le soft marche sur le TLM -> Il doit marcher sur le HW

Si SW marche PAS sur le HW -> Il ne doit PAS marcher sur le TLM

Si le SW marche sur le TLM mais pas sur le HW ... AIE AIE AIE   

Donc un modèle TLM doit être fidèle ! Faire TOUT ce que le HW fait  
Il peut avoir plus de comportement que le HW mais ne doit pas devenir iréaliste


{% include links.html %}
