---
title: Introduction
sidebar: mydoc_sidebar
permalink: cours_ct.html
folder: mydoc/ct
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



{% include links.html %}
