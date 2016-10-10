---
title: Introduction
sidebar: mydoc_sidebar
permalink: mydoc_cea.html
folder: mydoc
---

# Cours 03/10/2016

SoC : 
	- Processeur + Cache
	- Bloc mem
	- Contrôleur Mémoire externe (RAM)
	- Bloc de calcul pour soulager/accélérer les calculs
	- Bloc Analogique 
	- Bus

Bus : Débit + Protocole

RAM : < 10Mo de Cache + 4 à 128 Go de cache

CPU : 

DSP :  -> contrôle temps réel
	+ Puissance de calcul 
	- Programmation complexe 

ASIC/IP : 
	+ Puissance de calcul, traitement réguliers, consommation 
	- Rigidité, Temps de dév, Peu adapté à un contrôle complexe

Analogique :
	+ Faible conso, Grande intégration
	- Bruit, Forte dépendance à la techno

MEMS : MCNC

SoC : Assemblement de plusieurs techno 

Comment réduire la consommation d'énergie :  
Conso => P = 1/2 * C * V^2 * f
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


{% include links.html %}
