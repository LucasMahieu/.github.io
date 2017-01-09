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


# Cours du 05/12/2016

## OUT OF ORDER EXECUTION

Pipeline classique INORDER : 

IF | DEC | EXE | MEM | WB

Dans le pipeline on ajout un étage : RENAMING entre DEC et EXE

IF | DEC | REN | EXE | MEM | WB

2 tables sont utilisées : le RENAME MAP et le FREE MAP

RENAME MAP = table de correspondance entre registre logiciel et registre
physique qui gère les dépendances

FREE MAP = par exemple une fifo de registres libres permettant d'être utilisé
pour le renomage


Dans le pipeline on ajout un étage : DISPATCH entre DIS et EXE

IF | DEC | REN | DIS | EXE | MEM | WB


## Prédiction de branchement 

Solution naive dans MIPS, POWER PC, PENTIUM ... 

On a une Branch History Table (BHT) qui est indexé par les 14 bits de poids
faible de PC. (taille de BHT = 2^14 bits)

On mémorise uniquement dans cette table si le branchement a été pris ou non la
dernière fois que ce PC a été exécuté.

Cette méthode peut poser des problèmes de collision...

Cette méthode est trop basique car avec une boucle for on a forcement 2 miss
prediction ... à la 1ere itération et à la dernière 

# Cours du 04/01/17

## Vector processors

Apparus dans les années 80 jusqu'en 90.
Puis réapparition avec les GPU.

### Flynn's Taxonomy 

Von Neumann Architecture = CPU + Memoire (fetch + exec)

Année 70, M. Flynn distingue plusieurs types d'architectures:

- SISD = Single Instruction, Single Data
- SIMD = Single Instruction, Multiple Data
- MISD = Multiple instruction, Single Data (Ça n'est pas utile, donc n'existe
  pas)
- MIMD = Multiple instruction, Multiple Data (cas de nos ordinateurs
  multi-coeur, mais nécessite de la synchro ... )

SIMD : C'est ce qui correspond au GPU ou au machine vectoriel 

Utilisation : 

Pour faire une addition entre 25 couples d'éléments, j'ai juste à créer deux
vecteurs de 25 éléments et de faire 1 seule addition vectoriel de ces 2 vecteurs.

Souvent pour faire les opération vectorielles, il faut de grand pipeline, donc
il y a une grande latence, mais aussi un grand débit.

Permet de faire des :

- opérations simple: C[i] = A[i] + B[i]
- opération avec indirection : C[i] = A[i] + B[D[i]]
- opération conditionnelle : C[i] = A[i] + B[i], si A[i]>0 par exemple

### Multimédia Extension (MMX)

Forcement 64 bits, donc 8x8 ou 4x16 ou 2x32

L'utilisation de ces extensions ou des processeurs vectoriels est très
spécifiques : 

- Video
- Audio
- Image Processing 
- Data encryption

## Architecture Very Long Instruction Word (VLIW)

Apparu dans les années 80

Pas mal utilisé pour tout ce qui ne peux pas consommer bcp, les switch router,
...

Instruction de taille 128 bits typiquement

CISC: (x86)
RISC: (ARM, powerPC, ...)
VLIW: (TI, ST, HP, ...)

Un VLIW c'est un RISC avec des instructions plus grandes et un pipeline plus
grand. VLIW = super RISC.

En VLIW : un instruction de 128 bits contiens dans les 128 bits plusieurs
instructions simples, qui seront exécutées en MÊME TEMPS.

Le HW d'un VLIW est assez simple car le HW ne prend pas du tout en charge les
hazard (problème de dépendance entre instructions, etc)

La sémentique de l'asm en VLIW étant différente, il faut connaitre la latence de
chaque instruction.

## Dynamic Scheduling : Thomasulo

Trouver le parallélisme possible entre les instructions à l'exécution. Avec
renommage de de registre et ordonnancement des instructions.

Le but de faire du Out-of-Order est de 

# Cours du 09/01/2017

## Avant 2004

Un système classique (avant 2004) est constitué d'un bus sur lequel vient se
connecter tous les autres périphériques 

## Après 2004

Intel a arrêter de faire augmenter la fréquence des chips pour démultiplier le
nombre de processeur

Si dans un système on démultiplie le nombre de CPU, disons `n` CPU.

Chaque CPU à besoin d'une "bande passante" `b` pour échanger des données avec la
mémoire.

Il faut donc que le bus ai une bande passante possible d'au moins `n * b`.

Admettons que notre cache par CPU ai un taux de miss de 10%, et qu'il fait un
accès mémoire tous les 5 instructions

Alors il y aura 1 instruction sur 50 qui fait un miss.

Sachant qu'il y a 8 mots par ligne.

De plus, Le CPU tourne à 2 GHz mais le bus tourne qu'à 800MHz

Se que l'on constate c'est que la latence à une forme exponentiel et explose au
alentour de 10% = 1 requêtes tous les 10 cycles

## Les NoC

Pour remédier à cela, création des NoC

Utiliser des architectures réseaux qui ont de bonnes propriété: par exemple le
thor.

### Vocabulaire 

- _Message_ is a basic communication entity
- _Flit_ is a basic flow control unit
- _Phit_ is the basic unit of the physical layer
- _Direct Network_ each switch connecter to a node
- _Indirect Network_ with switch not connected to any node
- _Hop_ is the basic communication action from node to switch or from switch to
  switch
- _Diameter_ is the length of the maximum shortest path between any two nodes
  measured in hops
- _Routing_ distance between two nods is the number of hops on a route
- _Average_ distance is the average of the routing distance over all pair of nodes

### Basic switching techniques

- Circuit switching : Il existe un chemin entre source et destination, et le
  message utilise ce chemin.
- Packet switching : Chaque packet connait la destination et chaque packet est
  envoyé indépendamment dans le réseau.
- Store and forward packet switching : Tous les packets sont mémorisés dans tous
  les switchs.





{% include links.html %}


