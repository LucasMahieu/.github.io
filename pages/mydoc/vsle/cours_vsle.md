---
title: Notes de cours
sidebar: mydoc_sidebar
permalink: cours_vsle.html
folder: mydoc/vsle
---

# Cours du 12/10/16

[Lien du cours](http://chamilo2.grenet.fr/inp/courses/ENSIMAG5MMVSE/document/TRANSPARENTS/12.10.16.pdf?cidReq=ENSIMAG5MMVSE&id_session=0&gidReq=0&origin=)

Un bug ça coûte cher, mais peut etre moins cher que de la vérif.   
Exemple : 
	- Avion = Énormes vérif et controle et norme
	- Voiture = Pas de normes ... mais ca commence un peu
	- IoT = Pas de vérif car temps de vie "trop court"

Difficulté de l'embarqué et de devoir tester avant et en dehors de son contexte normal d'execution.  
Comment savoir si le système fonctionne comme on a prévu qu'il fonctionne.  
Mais il est aussi difficile de savoir ce que l'on aimerai que le système fasse.  
La vérification c'est une comparaison entre: 
	- La déscription entre ce que je veux 
	- Et l'implémentation du système  
Si non égale, se poser des questions, si oui c'est pas forcement génial !   

Prenons un exemple : 
	* Système de stabilisation d'un avion  -> les mouvement de l'avion doit être pris en compte pour que l'objectif soit atteint de façon sûr. 
	* ABS d'une voiture -> la voiture dois réagir correctement en sécurité 
	* Téléphone mobile faisant de la video : Correct = ? -> faut juste que ce que l'on voit soit "beau"
	* Carte électronique -> il faut connaitre les attaques pour pouvoir se défendre.

__Statique ou dynamique ?__

Statique = Regarder le programme sans le faire tourner
Dynamique = tout le reste : test à l'exécution, debug etc ...

Est ce que l'on peut savoir si il y a une division par zéro ? 
	* Peut pas être trouvé statiquement !   
Mon programme a t il besoin d'une mémoire finie ?  
	* Peut pas trop être détecté en statique, sauf si pas d'allocation mémoire, ni de récursion
Mon programme aura terminé en moins de 3 secondes ?  
	* Indécidable statiquement 

Le type de réponse d'un test de validation serait du type :   
	* Il existe un morceau de code qui plante
	* Il pourrait y avoir un morceau de code qui fait planter
	* Pour TOUTES les entrées possible, mon système est correcte.


Pour savoir si un erreur est accessible par un programme il faut : 

* Graph du programme  
* Graph d'une condition à respecter  

* Faire le produit synchrone des deux  

* Regarder plus finement les liens menant à un état d'erreur pour savoir les transitions sont possibles, et donc si l'état d'erreur est accéssible 

# Cours du 19/10/2016
[lien du cours](http://www-verimag.imag.fr/~raymond/edu/sleverif/slides-verif-sle-po.pdf)

## Introduction
3 notions de corrections d'erreurs:   
- Propriété générique: débordements arithmétiques, famine, bocage, etc ...   
- Propriété spécifique: Overflow, etc ...
- Propriété runtime: manque de mémoire, etc ... 

On peut essayer de raisonner par fonctionnalité : Modèle Fonctionnel   
- S'affranchir du système concret, pour juste raisonner sur les fonctions du sytème    
- L'exécution d'un système réactif est essentiellement une séquence de réaction entrées/sorites:   
  - Si indéterministe : relation e/s
  - Si déterministe : relation e -> s

Plan du cours:   
- Rappel sur Lustre   
- Exprimer/Vérifier des propriétés dynamiques: les quelles ? limitations ?   
- Abstraction finie   
- Technique d'exploration des systèmes finis  
- Des exemples, exercices, etc ... 

## Language Lustre
__Pourquoi Lustre ?__   
Simple, proche des maths, vision par flow de données

Types et opérateurs:   
- Bool, int, real   
- and, or, not, +, -, * , /, if-then-else  
- pre  
- '->'  

Hystéresis:   
Pour ne pas osciller on utilise un système d'hystéresis = seuils de tolérance décalés  
Pour passer de l'état 1 à l'état 2 la condition ne sera pas la même que pour passer de 2 à 1.  

## Définir les Propriété fonctionelle

Propriété fonctionnelle = ensemble de comportements correct   
Vérifier un propriété fonctionnelle = Vérifier que l'ensemble des comportements possibles est inclus dans l'ensemble des comportent corrects  

Propriété d'invariance(sureté):  
- Propriété d'état:  
	- Une configuration (ou état) d’un systéme est une valuation particulière des entrées, sorties et mémoires internes du système.  
	- Une propriété d’état est une relation (i.e. un ensemble) de configurations.  
- Propriété de sureté:  
	- Une propriété de sûreté exprime le fait qu’une propriété d’état est invariante au cours du temps.  
	- Ou, de manière équivalente, qu’une configuration (redoutée) n’arrive jamais  
	- n.b. le mot anglais est ≪ safety ≫ 

## Modèle équationnel

Système réactif est caractérisé par S = Ensembles des entrées, S = Ensembles de ces sorties, M = Ensemble des mémoires intérnes. Si ce système est déterministe alors il peut etre modélisé par (E,S,M,Mo,f,g).

Mo = Mémoire initiale  
St = f(Et, Mt) est la fonction de sortie  
Mt+1 = g(Et, Mt) est la fonction de transition  

Grâce à Lustre, c'est assez simple.







{% include links.html %}
