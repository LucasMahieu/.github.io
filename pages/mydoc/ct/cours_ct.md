---
title: Introduction
sidebar: mydoc_sidebar
permalink: cours_ct.html
folder: mydoc/ct
---

# 07/10/16 : Conception de système embarqués contraint sous RTOS à cloisonement

Depuis 2008 chez ELSYS Design

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

### Système Temps-Réel 

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

### Problèmatique de la programmation en contexte temps-réel 

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

#### Différent temps réel

- Temps réel "mou" :
	- Les contraintes de temps peuvent de temps en temps ne pas être respecter
	- La performance moyenne est prise en compte
- Temps réel "dur" :
	- doit TJS TJS respecter les contraintes de temps 
	- Pire cas est pris en compte

#### RT-OS
Temps de réponse garanti:

- Points clés :
	- Algo d'ordonnancement 
	- Temps de latence des irq
	- Temps de latence de changement de contexte
- Priorités des tâches et des interuptions
	- Traitant d'irq minimalisé en context d'irq
	- On peut reporter l'irq dans des tâches de haute prioritéo

#### Quelle RT-OS

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

### Comment mettre en oeuvre un tel système

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

### Notion de cloisonement 
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

### Type de cloisonement

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

### Scheduling en env cloisonné

Quelle prio pour chaque cloison ? 
Les cloisons doivent respecter les exigences RT
Que faire avec les taches de même priorité mais de cloisons différentes ?

- CHEZ INTEGRITY:
	- Prioritized
		- Tache e plus haute priorité et qui est préte, obtient le CPU
	- Preemptive
	- Enhanced Partition Scheduler
		- La tâche de plus haute priorité de la partition temporelle courante obtient le CPU.


# 14/10/16 : Mathlab: Polyspace

### Introduction
Polyspace depuis 2010 dans la vérification de code

3500 salarié
900 SW eng
90 products

Polyspace Dev Team  
- 30 eng  
- Paris + Montbonot  
- Aeronautique  
- Automobile  

Interprétation abstraite créé par M et Mme Couzau au 3ème étage de l'ENSIMAG
Outil d'analyse abstraite = Théorie qui prend en entrée un programme qui va de manière automatique va prouver qu'il n'y aura pas d'erreurs à l'exécution du programme

Programme = Ensemble de C/C++ qui compile 
Automatique = L'utilisateur ne dois pas intervenir 
Prouver = se baser sur une base mathématique solide
Réponse = Elle doit etre 'OUI', 'NON' et 'Peut être', le dernier choix est OBLIGATOIRE (c'est prouvé)
A l'exécution = Quand le programme s'exécutera en condition réelle.

### Exemple

* Ariane 5: Overflow
* Missile patriote: Erreur de float de 10-4% -> 137 métres
* Panne électricité au USA: data race
* Volvo crash: capteur accumule de l'imprécision 

### Les bases de l'interprétation abstraite
Pas la peine de tout connaitre dans un programme, il suffis de connaitre parfaitement les informations que l'on a besoin

Grille d'interprétation de signe par exemple   
    
						            Top   
						         <=      >=0   
							<0      =0       >0   
						          Bottom   
    
On joue alors le programme non plus en essayant de savoir TOUT mais utilisant un élement dans la grille
Les propriété que l'on utilise peut être divers:    
- Signe   
- Intervals   
- Octagons   
- Polyhedre   
- Disjonctions   

### Enjeux techniques

- Programme avec pointeur
- Programme avec structures
- Tableau
- Complexe control flow
- C++

Exemple avec des pointeurs:

- Erreur de déréférencement de pointeurs
	- Pour chaque déréférencement, il faut vérifier si le pointeur est valide
	
Calcul d'alias = Ensemble de variables qui ont des relations entre elles
ex : 
``` 
	  P ->  X
	  |---> Y  
```
Signifie que P peu pointer vers X ou Y et c'est tout 


Contexte insensitive : ne différencie pas les contextes d'appels de fonction : résultat assez large
Contexte sensitive : prend en compte les contextes d'appels de fonctions, plus stricte

Flow sensitive : Idem avec les if etc 
Flow insensitive : 

Le sensitive est trop couteux ... donc en pratique on fait du insensitive et on essaye de raffiner 

Algorithme de Anderson:   
- écrire les points-to (lien de pointeurs) = calcul d'alias  
- Il faut poser les équations.
- Il faut résoudre une équation de point fixe.  
- Complexité en ^3.  

Algorythme de Steensgaard:  
- Quand on fait le graph des alias, on le fait plus qu'une fléche qui part d'un pointeur, vers une boite contenant toutes les possibilités.   
```
                                    _
x---->  y                  X ----> |Y|   
|---->  z        devient           |Z|   
                                    -   
```

- Cette modification permet de réduire la complexité de n^3 à n^1
- Mais cette méthode ne permet pas de manipuler des tableaux ou des stuctures

Technique maison de chez MathWorks:   
- Vue de graph imbriqué   
- Pour les tableaux, on peut mémoriser que le pointeurs est dans une case sans savoir la quelle   

Pour prendre en compte les tableaux et les structures l'aglo se complexifie : de n^1 à n^2 

### Enjeux technologiques
Il faut que le produit soit intéressant, simple, efficace à utiliser.   
Pour réussir, on se pose les questions suivantes:

* Que va se service du produit ?
	* 1) SW dev
	* 2) SW eng
	* 3) Quality eng
* Chaque utilisateur ont des utilisations différentes
	* 1) Facile à utiliser, quelques clics, pas contraignant.
		* Prendre en compte le fait que le code testé ne sera pas forcement entièrement présent, ou pas tout doit être testé
		* Différent code généré par des compilo exotique 
		* Faire des intérprétation dans les modèls
		* Intégration dans un IDE, git etc
	* 2) Utilisable de facon efficace et conviviale 
		* Interval pour variable, points-to pour les pointeurs, relations entre variable
		* Code non attégnable 
		* Pour chaque RTE, OUI, NON, PEUT-ETRE
		* Prise en compte de bcp de faute "classique" : Overflown, NULL, Not Initialized, etc ...
		* Coloration
			* Vert = C'est SUR que le code est safe.
			* Rouge = C'est SUR que le code génére une erreur à l'exec.
			* Gris = Code mort
			* Orange = Ce morceau de code PEU poser problème (PEUT-ÊTRE...)
			* Violet = Violation de règle de codage : MISRA-C/C++
		* Information au passage de souris: intérvale d'une variable etc 
	* 3) Traiter les résultats de façon pas trop fastidieuse
		* On peut guider l'annalyseur en lui forcant la main au vert sur certain point
		* Tools 
		* Métrique de la qualité d'un code 
* Comment faire pour que l'outil fonctionne pour de vrais cas, qui ont beaucoup de LoC
	* Exemple : 200,000 LoC  ->  la plus longue chaine d'alias peut être de 16122 case mémoire; 5536 set d'alias 
	* Pour réduire la complexité, on peut faire du SSA [Static Single Assignement](https://fr.wikipedia.org/wiki/Static_single_assignment_form)

### Conclusion

Conférence POPL/PLDL = Meilleur conférence en info théorique   
en 2016 -> 35 papier sur les traitements de pointeurs   
De plus en plus de floatant -> NaN, infinity, subnormals -> Problèmes complexes  
On peut pas avoir tout vert, il faudra forcement passer par des oranges  

Anecdote :   
Livre sur le C++  

{% include links.html %}
