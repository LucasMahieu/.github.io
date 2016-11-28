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

# Cours du 07/11/16

### Etude de cas

[lien des slides du cours](http://chamilo2.grenet.fr/inp/courses/ENSIMAG5MMCEAMC/document/multimedia.pdf?cidReq=ENSIMAG5MMCEAMC&id_session=0&gidReq=0&origin=)

# Cours du 28/11/17

Cours de petrot, début du cours d'archi des machines modernes
[Lien du site du cours](https://ensiwiki.ensimag.fr/index.php/Transparents_du_cours_d%27Archi_3A)

## Super pipeline vs superscalar 

Super pipeline = plein de petit étage 

Superscalar = plusieurs instructions en parallèle 

Exemple du MIMPS Superscalar:

2 pipelines en parallèle (pas 2 identiques)

Une partie fait les opérations de type : 

- opération entre registres  
- opération entre registres et constante (immédiat)

Une autre partie fait :

- Opération d'accès à la mémoire

Or, 1 instruction sur 5 est un accès mémoire, donc pas forcement bien répartie 

Pour qu'un superscalar puisse exécuter 4 instructions dans le même cycle,   
il ne faut pas que les 4 instructions soient dépendantes.  

- Dans un processeur VLIW, le compilateur devra délivrer des groupes de 4
  instructions indépendantes
- Dans un processeur 'in-order superscalar', c'est l'HW qui va exécuter au
  maximum 4 instructions si elles sont indépendantes. Sinon, insert des stall
- Dans un processeur 'out-of order superscalar', c'est l'HW qui choisi 4
  instructions indépendante qu'il pourrait exécuter mtn.

Exemple slide : 6 

Avec processeur MIMPS classique : 7 cycles

In order : 5 cycles

| Cycle | Pipeline NOIRE | Pipeline JAUNE |
|-------|--------|---------|
|   1   |   ...  |  lw $6  |
|   2   | add $5 |   ...   |
|   3   | sub $9 |  lw $7  |
|   4   | add $3 |  sw $5  |
|   5   | add $11|   ...   |

Out of order : 4 cycles

| Cycle | Pipeline NOIRE | Pipeline JAUNE |
|-------|--------|---------|
|   1   | sub $9 |  lw $6  |
|   2   | add $5 |   ...   |
|   3   | sub $3 |  lw $7  |
|   4   | add $11|  sw $5  |

Execution time = Number of instructions * CPI * cycle time 

## ILP : Instruction Level Parallism

Pipeline CPI = Ideal pipeline CPI + Structural stalls (= stall after lw) + data
hazard stall (= for data which result of prior instruction, tjs dans le pipeline)
+ control stalls (= par exemple, les instructions exectuter pour les
  branchements)

### Dépendances 

#### Data dépendance 

```
I: add r1, r2, r3
J: sub r4, r1, r5
```

l'instr J est __data dépendant__ de l'instr I

#### Anti-dépendance 

```
I: sub r4, r1, r3
J: add r1, r2, r3
K: mul r6, r1, r7
```

J est anti-déprendant à I, ce n'est pas une vrai dépendance, dépendance de nom

#### Output dépendance 

```
I: sub r1, r4, r3
J: add r1, r2, r3
K: omul r6, r1, r7
```

J est output dependant car après ces deux instructions la valeur de r1 doit tjs
être la même

Pour les dépendances de nom, on pourrait faire utiliser une IP de renomage des
registres pour en avoir plus que 32 sans pour autant augmenter la taille des
instructions. 











{% include links.html %}


