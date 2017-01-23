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
- Approximately Timed (AT) : Temps précis induits par la microarchi,
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

Du soft erroné peut être simulé de 2 façons : 

- ISS: Il y a un context switch toutes les instructions ou toutes les X
  instructions, dans ce cas, on peut "camoufler" des erreurs en simulation,
alors que la vrai puces aurait plantée.
- en NATIF: Il faut faire des CPU_RELAX() pour que le temps passe et pour que
  les autres processus puissent s'executer. Donc il peut également il avoir des
camouflage d'erreur.

# Optimisation des performances

## Transaction bloc

Pour écrire des données à un composant sur le bus, on fait des socket.write(..)
Or, si on fait un truc basique, pour chaque mot à envoyé, il faut faire un
requete au bus, qui doit décoder l'adresse, etc etc ... 

On pourrait faire un socket.block_write(...) qui demande de faire un transfert
depuis une adresse d'une quantité X. Ceci permet de faire 1 seul décodage
d'adresse et donc de gagner pas mal de temps.

Cela rend donc l'écriture d'une zone mémoire atomique.


## Timing approximé 

Context-switch est très cher

Donc on évite les wait.

Pour faire avancer le temps, on fait des wait(petit_temps), à chaque tour de
boucle.

__Solution : Quantum Keeping__ On utilise un compteur sur 64 bits qui s'incrémente à la place du
temps. Et de temps en temps on fait un wait de ce compteur qui à simuler le
temps, et on remet le compteur à zéro. Cela fait faire un context-switch 
bien moins souvent.

## Parallélisation de SystemC

Les systèmes sur puces sont parallèles or SystemC n'a qu'un seul processus.
Mais SystemC a qu'une notion de processus.

__Solution naive:__ Chaque SC-THREAD créé des p_thread -> bcp de p-thread donc
ne passe pas à l'échelle

__Solution un peu mieux:__ N processus = N processus SystemC

__Solution testé__ On pourrait analyser le code et créer des processus qui
exécute du code qui n'a pas de variables partagé. En théorie, bien. En pratique,
impossible car bcp trop de dépendance entre les processus.

# Estimation de consommation d'énergie et de température 

Modélisation de la plateforme en une machine à état à 3 états :

- Run : 3 Watt
- Idle : 1 Watt
- Wait : 0 Watt

Donc on fait l'intégral sur le temps passé dans chaque état pour connaitre la
conso sur un temps donné


Pour la température, on fait la somme de tout ce qui ajoute de la chaleur et de
tout ce qui en enlève.


# Exam

- Questions de cours C++, SystemC/TLM, intervenant extérieur.
- Question temps simulé / temps wall clock
- Composant pour améliorer le TP3 : améliorer le memset
- SC_MODULE, SC_THREAD, SC_METHOD, wait, notify, ...
- hal.h
- Principe de TLM2
- Confusion SW et HW

# Récape TP

## TP1

Générateur -> Bus -> Memory

## TP2

Module LCD en plus, et la ROM


## TP3 

- Intégration du logiciel embarqué
	- ISS 
	- Native
- hal.h -> soft embarqué, doit être compilé avec le compilateur embarqué

# Intervenant ST:   

Jerome CORNET


## Dernière nouvelle de l'industrie du semi-conducteur

### Conduite autonome 

Ce qui se passe naturellement, pour pouvoir traiter toutes les données 
c'est de passer d'un CPU à GPU, puis Neural PE, traitement d'image.

Mais il y a aussi bcp de contraintes vis à vis de la quantité de traitement de
données.


Sureté  = Ne pas engendrer de dégâts en cas de défaillance.

Sécurité = Être protégé de menace extérieur.

Ce qui induit des chips gros, avec bcp d'IP très complexes, des moyens de
communications nombreux et complexe, mémoire (importante).

### IoT

Beaucoup de cas d'utilisation possible : utilisateur privé, industrie, medical,
...

Ceci induit une très forte connectivité.

Ainsi que des problématiques de consommation 

Induit l'augmentation du nombre de petits chip.

Des chips petits, sans mémoire, seulement quelques IP, ...


ISO 26262 c'est la norme de safty pour l'automobile 

## Prototypage virtuel

Émulation = utilisation de logiciel qui existe déjà pour l'utiliser de la MEME
façon qu'avec le HW d'origine. 

Virtualisation = exécuter du soft mais dans le but de concevoir du SW non
existant.

En virtualisation, on reproduit chaque petite tâche donc on peut trouver de
vrais bug liés au temporel et à l'ordre.

Sert à faire du développement de logiciel embarqué en avance de phase.

Vu que c'est en avance de phase, ça permet de trouver des contradictions, des
ambigüités du cahier des charges.

En post silicium, permet de comprendre ce qui se passe dans le chip.

Peut servir au client pour lui permettre de l'aider à développer le reste de son
système.


Exploitation d'architecture = permet de ne pas s'intéresser aux données mais au
débit et au transport de données.

### Métier de virtualisation à ST

Faire des modèles

Faire des couches au dessus de TLM (comme ensitlm) pour faire des choses
réutilisable pour en faire des legos

## Grandes tendances 

Fonctionnalités nouvelles:

- Simulation d'horloge : ex. Basse consommation
- Simulation de voltage
- Simulation des PIO Muxing (plusieurs fonctions sur un I/O)

Injection d'erreurs :

- Rien de plus simple, il suffis de changer quelques lignes dans le modèle.
- Simuler une intrusion dans le système. (changement dans la DDR)
- Tester des cas limites (cas des 16 collisions dans ethernet)

Intégration multi-système:

- Faire plusieurs modèles sur plusieurs simulateurs et les faire communiquer
  ensemble.
- Ça serait un vrai plus pour exploiter les many-cores.
- Permet de faire des simu pour des clients avec des morceaux de chez ST, et des
  morceaux chez les clients.

Dans l'automobile :

- Simuler les réseaux de nœuds d'une voiture qui comprend bcp de uC


## Conclusion




{% include links.html %}
