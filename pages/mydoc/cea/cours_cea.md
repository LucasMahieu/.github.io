---
title: Notes de Cours
sidebar: mydoc_sidebar
permalink: /cours_cea.html
folder: mydoc/cea/
---

# Cours du 03/10/2016

* SoC : 
	- Processeur + Cache
	- Bloc mem
	- Contrôleur Mémoire externe (RAM)
	- Bloc de calcul pour soulager/accélérer les calculs
	- Bloc Analogique 
	- Bus

* Bus : Débit + Protocole

* RAM : < 10Mo de Cache + 4 à 128 Go de cache

* CPU : Unité de calcule  

* DSP :  -> contrôle temps réel
	+ Puissance de calcul 
	- Programmation complexe 

* ASIC/IP : 
	+ Puissance de calcul, traitement réguliers, consommation 
	- Rigidité, Temps de dév, Peu adapté à un contrôle complexe

* Analogique :
	+ Faible conso, Grande intégration
	- Bruit, Forte dépendance à la techno

* MEMS : MCNC

* SoC : Assemblement de plusieurs techno 

* Comment réduire la consommation d'énergie :  
* Conso => P = 1/2 * C * V^2 * f
	- ASIC 
	- RAM : SRAM / DRAM
	- Techno : 
		- Fils moins long
		- Petit, densité
		- Compromis en taille des transistors(performance) et consommation
	- Archi du système :
		- Bus -> longueur de fils -> charge du circuit diminu
		- Paraléliser -> on peut diminuer la fréquence et diminuer la tension
	- Tension : Lié à la techno
	- Amélioration combinatoire -> éviter les glitchs
	- Simplification du SW

# Cours du 10/10/16

[lien du cours](http://chamilo2.grenet.fr/inp/courses/ENSIMAG5MMCEAMC/document/SLE_SoC_infrastructure.pdf?cidReq=ENSIMAG5MMCEAMC&id_session=0&gidReq=0&origin=)

## Bus 

{% include tip.html content="Un bus système sert à assurer le transfère de données entre les organes d'un SoC" %}

{% include note.html content="Si on veut utiliser 2 CPU et 1 mémoire avec un bus <br/>
Dans ce cas, l'instruction n'est plus fetch en 1 cycle (plus forcement)"%}

* Connectique:
	* Bus adresse
	* Bus donnée
	* Bus de contrôle
* Physique:
	* Protocole d'échange

Comment gérer plusieurs maitre et plusieurs esclave ?  
__Bus Trois Etat__  
* Point positif:
	* Grande modularité
	* Connectivité maitre-esclave simple
	* Observabilité simple
* Point négatif:
	* Fréquence de fonctionnement faible à cause de la capacité des 3 états
	* Test complexe 
	* Grande longueur des connexions
	* Layout délicat: Antenne

## Controleur de bus

{% include tip.html content="Le contrôleur de bus répond aux requêtes des maitres. Il donnes accès au bus à une demande selon<br/>	Une politique de priorité <br/>	Une gestion temporelle du bus" %}

Exemple d'arbitre pour un bus :   
{% include image.html file="exemple_arbitre.png" alt="Arbitre de bus" caption="Exemple d'arbitre" %}

Conclusion du petit exercice: on est limité en nombre de maitre et d'esclave.

La solution consiste à _hiérarchiser les bus_

* Conversion de protocole par un Bridge
	* +: pas cher, pas de matériel
	* -: peut vraiment bloquer en terme de performance si plusieurs maitre veulent communiquer en même temps
* Stockage intermédiaire dans une SRAM
	* +: Le maitre écrit dans la RAM, et se préoccupe plus de rien, libère le bus
	* -: Cher

## DMA : Direct Memory Adress

{% include tip.html content="DMA = C'est un automate de transfert automatique de données d'un périphérique vers la mémoire" %}

Burst: plutôt que de faire une requête et d'attendre la donnée après plusieurs cycles d'attente, on fait un demande de n données d'un coup.

Caractéristique du bus:
* Nombre de caneaux
* Gestion du bus
* Transferts 
	* Groupé : Burst
	* Auto-incrémenté
	* Canaux chaînés
* Mode de transfert 
	* FIFO
	* Taille de données
* Interruption 
	* Fin de transfert
	* Erreur
* Priorité des canaux

## Exemple de bus

#### CoreConnect 

Proposé par IBM

## Mémoire

[Lien des slides](http://chamilo2.grenet.fr/inp/courses/ENSIMAG5MMCEAMC/document/SoC_memoire.pdf?cidReq=ENSIMAG5MMCEAMC&id_session=0&gidReq=0&origin=)

{% include tip.html content="Les mémoires sont des tableaux de points mémoire assemblés en ligne de mots" %}

C'est la technologie des points mémoires qui détermine les performances d'une mémoire.  

* Densité 
* Rapidité 

Décomposition de mémoire en bloc car c'est impossible d'avoir un gros seul bloc  
Sinon elles serait trop lentes  

# Cours du 17/10/16
La capacité induite des lignes d'information fait que l'on ne peux pas utiliser le même modèle de mémoire si elles sont trop grandes.   
{% include image.html file="mem.png" alt="plan mémoire" caption="schéma d'un plan mémoire" %}

### SDRAM : Synchronus Dynamic RAM
C'est un transistor et une capacité:  
Points positifs:   

* Grande densité car petite
* Pas chère

Points négatifs:  

* Pert la valeur à chaque lecture car le condensateur se décharge.
* A cause des fuites de courant, le condensateur se décharge, le niveau passe sous le niveau 1.

Il faut donc faire constamment des lectures et re-écriture pour mémoriser les données.

Les SDRAM doivent être composer de plusieurs petites mémoireso

__Résumé__  

+ Densité élevée
+ Débit élevé
+ Coût faible

- Grande latence
- Technologie spécifique
- Consommation: pas terrible car il fait bcp lire et re-écrire
- Non synthétisable

### ROM

### Flash

### Registre
16 transistors par point mémoire, très chère 

### Les caches


{% include links.html %}

