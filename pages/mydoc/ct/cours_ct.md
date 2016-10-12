---
title: Introduction
sidebar: mydoc_sidebar
permalink: cours_ct.html
folder: mydoc/ct
---
# Conception de système embarqués contraint sous RTOS à cloisonement

Depuis 2008 chez ELYSE Design

- Localisation:
	- USA
	- France
	- Europe
- Clients:
	- Aerospatial 
	- Defense 
	- Energie 
	- Medical
	- Miltimedia 
	- ...

Entretien technique, Excellence technique.

## Système Temps-Réel 

Interface plus simple avec le matériel:
	- Gestion de taches
	- Gestion des irq
	- Temps 
	- Communication
	- Ressources 

Plusieurs taches qui paraissent s'exécuter en même temps sont en fait exécutés séquentiellement.
-> Il faut donc ordonnancer les tâches.

Si c'est multi-coeur, plusieurs tâches peuvent s'exécuter en même temps.

- Concurrence :
	-  Priorités différentes, quelle politique ?
		- Run to completion
		- Utilisation de "pthread_yield"
		- Time Sharing 
		- Tick système 
- Préemption :
	- Scheduler peut suspendre une tâche
	- Problème de réentrance: interrompre une oppération qui modifie une variable en mémoire
	- Synchronisation 
- Inversion de priorité :
	- Permet de donner une plus grande priorité à un tâche qui à la ressource.
- Dead Locks :
	- Tjs prendre les ressources dans le même ordre

-> Systèmes temps réels :   
Capacité de traiter en un temps déterminer un ensemble d'évènements   
	- Sans perdre d'évènements  
	- Déterminisme de l'ordre de traitement de ces évènements  
Temps-réel ne veut pas forcement dire performant (pas tjs)  

## Problèmatique de la programmation en contexte temps-réel 

- Zones critiques
	Desactivation des intéruptions => Attention !!
- Solution 
	- Eviter au plus de désactiver les intéruptions
- Optimisation du compilateur 
	- Plus efficace en activant l'optimisation du compilo
	- Mais on peut avoir de mauvaise surprise : 
		- Debug pas à pas chelou
		- Volatile pour les accés registres 
		- Volatile pour une variable partagé entre contexte de tâche et contexte d'intéruption

### Différent temps réel

Temps réel "mou" :
	- Les contraintes de temps peuvent de temps en temps ne pas être respecter
	- La performance moyenne est prise en compte
Temps réel "dur" :
	- doit TJS TJS respecter les contraintes de temps 
	- Pire cas est pris en compte

### RT-OS
Temps de réponse garanti:

- Points clés :
	- Algo d'ordonnancement 
	- Temps de latence des irq
	- Temps de latence de changement de contexte
- Priorités des tâches et des interuptions
	- Traitant d'irq minimalisé en context d'irq
	- On peut reporter l'irq dans des tâches de haute prioritéo

### Quelle RT-OS

- Cout
- Fonctionnalité
- Performance temps-réel
- Encombrement
- Offre logiciel
	- Protocoles de com. (TCP/IP)
	- Applications (serveur web,...)
- Outils de développement et mise au point
	- IDE
	- Debugger
	- Analyseur de temps-réel
	- Simulateur

ASP = Architecture Support Package 
BSP = Board Support Package

## Comment mettre en oeuvre un tel système

- Cycle en V mais bon ... En théorie :  
	- Invalidation des hypothèses de départ
	- Changement des exigences en cours de route
	- Identification d'un problème technique
- Solutions ? Dérisqer   
	- Analyse de risques
	- Actions préventives
	- Développement itératif  
	- Envisager le pire cas
		- Périodicité des signaux
		- Exactitude du pilotage
		- Freq des irq

## Notion de cloisonement 

Exemple du point de vente:

- IHM 
	- OS non temps-réel
	- IHM peut planter, pas grave
- Un module de paiement
	- Sans OS, certifié
	- Module dédier et cértifié -> NE DOIT JAMAIS PLANTER

=> Enjeux : Rassembler les deux fonctions sur un seul materiel

	- Passage de cloisonnement "matériel" à cloisonnement "logiciel"
	- Réduction des couts

## Type de cloisonement

- Cloisonnement mémoire 
	- Protection d'un espace cloisonné contre les débordements mémoire d'une autre cloison
	- Chaque proc est cloisonnée dans son cloisonnement
	- Utilisation de la MMU du uP pour exposer un espace d'adr virtuel
	- Tous accès interdits lèvera une exception, traité par l'OS
- Cloisonnement temporelle
	- Disponibilité d'une quantité garantie de CPU à une partition temporelle
	- Découper l'exec en 'Time Slices" sur lesquels on sait parfaitement quel tâches peut utiliser les ressources

- OS Classique 
	- Mémoire partagé 
	- IPC, messagerie, signaux
	- Mutex ...
- OS Cloisonnement 
	- Aucun risque de corruption d'une autre cloison
	- Pas de mémoire partagé
	- Pas de communication possible entre les cloisons : COMMENT FAIRE ? 

Exemple : Dans INTEGRITY 

	- Partition "Virtual Address Space"
	- Memory région : mem partagée mappé dans les deux VAS
	- Com bidirectionnelle synchrone ou asynchrone
	- Semaphore : synchro entre les VAS

Du coup en cas de crash ? 

	- Memory Region : Données corrompues, accès concurrents (RO, WO, semaphore)
	- Connection : Corruption des données; Absence de réponse à une requête
	- Semaphore : Blocage de la ressource
INTEGRITY à mis en place un "Resource Manager"

## Scheduling en env cloisonné

Quelle prio pour chaque cloison ? 
Les cloisons doivent respecter les exigences RT
Que faire avec les taches de même priorité mais de cloisons différentes ?

- CHEZ INTEGRITY:
	- Prioritized
		- Tache e plus haute priorité et qui est préte, obtient le CPU
	- Preemptive
	- Enhanced Partition Scheduler
		- La tâche de plus haute priorité de la partition temporelle courante obtient le CPU.




{% include links.html %}
