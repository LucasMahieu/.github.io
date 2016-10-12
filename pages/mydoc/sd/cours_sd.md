---
title: Introduction
sidebar: mydoc_sidebar
permalink: cours_sd.html
folder: mydoc/sd
---

# Cours du 12/10/16

Récap sur séance précèdent:  
Quoi ?  
Pourquoi ?  
Différence avec Système Embarqués/Centralisés et les Système distribués :   
	- Communication entre processus
	- Mémoire partagée 
	- Absence d'horloge partagée
	- Tolérance au faute
Comment faire pour utiliser ces systèmes ?    
* Spécification :  
	- Communications ? 
	- Synchronisation ?
	- Consensus ?  
* Hypothèse
	

## Détection de fautes

* Détection de fautes parfait :
	* Specs: 
		* Interfaces: notifie que le processus P a craché
		* Propriétés: 
			- Complétudes forte: Toutes pannes de processus sera détéctée
			- Précision forte: Une panne machine est détéctée comme panne processus
	* Hypothèse:
			- Panne franche sans aucune bornes
			- Communication: fiables, borne sur le temps de propagation
			- Temps de traitement: bornée et borne connue
	* Algo 

## Communication point-à-point fiable

* Spéc:
	* Interface:
		1. In : send(m,p) -> envoyer le message m vers le processus p
		2. out : deliver(m,p) -> parmet de délivré le message m émis par un processus p
	* Propriétés:
		1. Validité : si un processus correct émet un message m à destination d'un processus correct p alors ce dernier le délivrera.
		2. Intégrité : un processus ne peut délivrer un msg qu'au plus 1 fois et seulement si ce msg a été émis par un processus.
	* Hypothèse:
		1. Pannes franches
		2. Autres modèles de pannes
	* Algo + Implem + Preuve

TCP est un exemple de canal fiable donc interdit dans nos TP.
On part d'UDP pour en faire un canal fiable.

__Ce qui est attendu pour le TP__   
1. Canal de communication fiable 
	* Hypothèse
	* Algo
	* Implémentation : basé sur UDP
	* Évaluation des performances de l'implémentation  
		- Débit (Netperf, ...)
		- Ping = Latence
2. Détecteur de faute
	* Hypothèse
	* Algo
	* Implémentation
	* Validation expérimental
{% include links.html %}
