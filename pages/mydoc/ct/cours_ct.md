---
title: Conférences Technologiques
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
- Aéronautique  
- Automobile  

Interprétation abstraite créé par M et Mme Cousot au 3ème étage de l'ENSIMAG [wiki](https://fr.wikipedia.org/wiki/Interprétation_abstraite)    
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
   
		  P -->  X
		   \__> Y  
    
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

Algorithme de Steensgaard:  
- Quand on fait le graph des alias, on le fait plus qu'une fléche qui part d'un pointeur, vers une boite contenant toutes les possibilités.   
       
{% include image.html file="Steensgaard.png" alt="Algo de Steensgaard" caption="Exemple d'algo de Steensgaard" %}

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


# 21/10/2016 : Marine National: Information quantique, 40 ans après ...

#### Il y a forcement eu des progres techniques qui ont permis le passage en pratique :  
- Progrès technique 
- Miniaturisation: Loi de Moore  
- Progrès industriel: Production de masse  

### Statut historique 

### Évolution de la pensée: du corpuscule au quantique  

#### Max Planck et la radiation des corps noir  
- Change de couleur  
- T = f(v)   

#### Albert Einstein : l'effet photo-électrique  
- Généralisation à la lumière    
- Effet photovoltaïque et photo-électrique  
- Contradiction des lois classiques de la thermodynamique car il y a un seuil : fonction de travail  = theta = hv  

#### Bohr : modèle atomique   
- Quantification des niveaux atomiques  
- Spectre d'émission pour passer d'un niveau à un autre, émission d'un photo   

#### Trous d'Young   
- Généralisation aux particules (y, e-)   
- Interférences constructives et destructive   
- Dualité onde corpuscule (e- dans les fentes d'Young)   

#### Erwin Schrödinger   
- L'atome est une onde, son état est une fonction d'onde qui est présente dans l'Equation de Schrödinger  
- Probabilité (t, r)  

### Mécanique quantique: Notions fondamentales  

#### Interprétation de Copenhague  
- Un système est décrit par une fonction d'onde  
- Cette fonction d'onde est décrite par l'équation de Schrödinger  
- Probabilité (en mécanique quantique, on parle tjs de proba), mesure (La mesure détruit des choses) et décohérence   
- Principe d'incertitude Dx.Dp > h/2   et De.Dt > h/2, on ne peut pas connaitre la vitesse d'une particule ET sa position, idem avec temps et énergie   

#### Intrication/Superposition quantique   
- État de superposition  

#### Chat de Schrödinger, paradoxe d'EPR   
- L'état du chat est en superposition : ``` (|DEAD> or |ALIVE> ) OR (1/sqrt(2)(|DEAD> + |ALIVE>)) ```
- Rôle de l'observateur, de la mesure   

#### Décohérence et cohérence  
- Temps de cohérence, temps maximum durant lequel l'objet est en superposition, durant le quel on peut profiter de l'effet quantique   

#### Mesure faible  
- Comment mesurer sans détruire ?   
- On mesure faiblement, sans trop changer l'état du système, mais un grand nombre de fois pour établir des statistique   

### Développement de l'information quantique   
Combinaison de facteurs  

#### Théorie CLASSIQUE de l'informatio: Shannon   
- Source, réception, bruit   
- Nyquist, 1928, Quantité d'information que l'on peut transporter   
- Hartley, 1929, Qualité de l'information  
- Shannon, 1948, Regroupement + approfondissement  
Même si on perd des informations, si le taux est < au capacité d transmission, avec des codes correcteurs d'erreur, on peut retrouver ~100%   

#### Les nanotechnologies: Feynman  
- Physicien électrique: projet Manhatten et Challenger  
- 1er transistor en 1947  
- 1er circuit intégré 1958  
- 1959: Possibilité de manipuler et de créer des objets nanométriques    
	- Concepte de nano-robots, nano-manipulateur  

#### Miniaturisation des transistores  
- Loi de Moore, loi purement économique   
- Dimension atomique: plus on se rapproche de la taille atomique plus il y a des problèmes : Ec devient > à KbT   
- Intégration 3D : Connections ?, chauffage ? Efficacité ?  Beaucoup de nouveaux problèmes  
- Problèmes calculatoires: Dans l'espace de Hilbert on arrive très très vite à des besoins de ressources gigantesque   

#### L'implatation mono-atomique   
- Réussir à implanter 1 seul atome  

#### Concept de machine : Turing  
- Automatisation des tâches  
	- Contraingnant   
	- Calculs plus complexes, nombreux  
	- Rapidités et qualités des décisions  
- Machine de Turing (virtuelle)  
	- Code, état initial , courant, etc

#### Concept du qubit   
- Etat classique  
	- 0 ou 1 - > le bit  
- Etat quantique  
	- 0 ou 1 ou 1+0 ou 0+1  -> le Qubit
- Niveau atomique   

#### Opération   
- Les opérations sont des mouvements dans l'espace 

#### Concept de clonage et téléportation   
- Non clonage   
	- Pas de copies identique d'un état quantique inconnue  
	- Seul les états originaux sont possible  
	- Pas de techniques classique de corrections d'erreurs   
	- Copies imparfaites  
	- En effet, copier c'est d'abords mesuré = détruire en quantique  
- Pas de téléportation, pas de transmission (superluminale)   
	- Pas de mesure précise possible (part d'incertitude)   
	- Pas de reconstruction d'état quantiques via des états classiques  

#### Nécessite d'un contrôle  
- Circuit adapté de mesure pour la quantique  
	- Electronique basses températures  
	- Mesure classique  
	- Archi complexe  
- Les mesures elles même doivent être différentes   
	- réflectométrie radio fréquence  
	- Ampli trans impédance  

### Status actuel  

#### Industrie vs. Académique   
- Deux point de vue différent   
	- __Industrie:__ L'industrie veut que ca soit peu couteux, simple, extensif et compatible avec leur production industrielle   
	- __Recherche:__ On cherche l'intérêt de la recherche
- Avantage / inconvénients:   
	- Pression des lobbys (D-Wave en 2011, "Quantum annealing", pas de superposition, pas de temps de cohérence)   
	- Pas forcement le plus intéressant, facile ou le mieux scientifiquement   

##### Différente approches et qbits
- Ces technos mélange plusieurs suport d'information : photo, e- etc 

#### Condition de (Bruce) Kane pour faire PRATIQUEMENT un Qubit   
- Conditions de bases :  
	- Définir le qubit  
	- Initialiser le système  
	- Déterminer un ensemble d'opérations universelles  
	- Avoir un temps de cohérence long  
	- Lire des information avec de grande probabilité  
	- Réaliser un grand nombre de qubits  
- Conditions délocalisées  
	- Propagés 
	- ??? 

#### Modèle de Qubit de Kane  
- Structure MOS en Si  
- Spin nucléaire (mémoire), électronique (qubit)

#### Qubit semiconducteur 
Boites quantiques  
	- Qubit de charge (Temps de cohérence = 100uS): Deux places accecible à un e-, si il est là alors on a un 1 sinon on a un 0.  
	- Qubit de spin (Temps = 1à5ms et jusque 100ms dans Si) : spin + -> 1 spin - -> 0  
	- Implantation mono ou bi-atomique / STM: N = 1-2, T = 45s, T refroidi > 1h   

#### Qubit supraconducteur  
- Molécule  T = 3ms  
- Jonction josephson T = 2uS  
- Diamant qui a des défaut avec les NV  
- Piège à ions : confiner des atomes grâce à des lazer  

#### Le graphène et autres mono couches  
Découvert en 2004 à Manchester  

#### Les matériaux topologique  
- Les isolants topologiques sont isolants en leur cœurs et conducteurs en surface.  
- Si on le coupe, on re crée des surfaces, il redevient conducteurs sur toutes ces surfaces  
- Longueur de cohérence = 300 à 600 nm à 300mK
- Peu être utiliser pour faire des Bus ou des Trains quantique  

#### Qubit volants  
- Avec des photons , pour les satellites ? 

#### Interaction localisé - délocalisé  

#### Projet et financement  
Australie: subventionné par US Army  
Europe : Pays-Bas, UK  
USA  
Canada  
Chine  
Russie  
Japon  

### Applications  

#### basé sur l'intrication des états 
- Calculs parallèles, vitesse accrue/ puce classique  
	- Pas de transmission de données car 2 éléments, même distant, connaissent les mêmes informations   
	- Gestion de trafic  
	- Médecine  
	- Astronomie  
	- Etude du génome humain  
- Algorithme de Shor: factorisation des grands nombres  
	- Classique réduction : calcul du PGCD  
	- Quantique : accélération  
	- (LogN)^3 au lieux de exp(log(N)^1/3)  

#### basé sur l'effondrement de la fonction d'onde par le phénomène de mesure  
- Inviolabilité, cryptographie  
	- Transmissions financière sécurisées (civil)   
	- Réseaux ultra sécurisés (DARPA, militaire)  
	- Transmission par satellite (Canada-Israél)

#### Codage et chiffrage classique 
- Classique : pour que ca soit plus dur, on augmente le nombre de bits   
- Quantique : va pouvoir aller bcp plus vite, il faut trouver d'autre moyen pour crypter de l'information   

#### Supériorité vs. Infériorité 
- Rapport de forces entre les nations

### Conclusions

Cohérence : les temps de cohérence ne sont plsu vraiment un problème aujourd'hui, les T sont de plus en plus grand   
Passage à l'échelle : Dépend des approches, mais en Si oui pq pas  
Déplacement de l'information: Comment faire communiquer les ordinateurs quantique entre eux  ?  
	- Fibre optique, mais sa qualité est des fois un problème    
	- Comment faire des Répétiteurs : Clonage ???  
	- Bus de Qubits ??  
Inviolabilité: beaucoup de recherche sur le bruit quantique et les mesures faibles  
Aspect stratégique pour la défense :   
	- Modernisation, automatisation et intégration des nanotechnologies ou des réseaux rendent les systèmes très complexe  
	- Comment maitriser cette modernisation et la protéger ces systèmes  

# 16/12/16 : BH Technologie, IoT, Jérôme DEGRYSE

Le partenaire des villes intelligentes

Start up, depuis 20 ans -> PME 45 personnes  
9M€ de CA, 50% de R&D  
Les clients sont les villes  
Présence Européenne  

- Optimisation de l'éclérage publique: Aider les villes à optimiser l'éclérage des villes
- Aide les villes à la récolte des déchets

## Optimisation de l'éclairage 

- Gestion du temps de l'éclérage
- Gestion de la puissance en fonction de l'heure de la journée 
- Relève d'incident sur les lignes 
- Cloud de supérvision 
	- Connaitre tous les informations des armoires de distibution
	- Envoyer des alertes en cas de problèmes
	- Mesure et traitement des données récoltées
	- Paramétrage via le cloud

Plusieurs projet concret : Troyes, Grenoble, Brest

### Résultats 

- Augmentation de la durée de vie du matériel
- Rentabiité en 3 et 5 ans
- 40% d'amélioration du traitement des pannes à grenoble

## Gestion de l'environnement 

### Le produit

"Sonde syren" 

Un capteur ultrason, robuste, étanche, Réseau IoT, envoie des infos de
remplissage prises toutes les heures, envoyées toutes les 6 heures.

Doit tenir 10 ans sans aucune intervention humaine

Permet d'optimiser les tournées de rempissage 

Application web sécurisée permetant de monitorer les données émis par les
capteurs et des données plus abstraite : nombre de fois que le conteneur a été
vidé, plein, transporté etc 

### Résultat 

- -20% de kilométre parcourus 
- -40% de temps de collecte gagné 
- 1/3 des durée de vie 

### Exemple concret 

- 2400 conteneurs équipés de ces capteurs
- Gain de 30 à 60% dans la durée de vie du matériel

## Objectifs 

Obtimisation des tournées pour ne pas que le calcul dure des heures. 

Question : 

- Durée de vie de la batterie
- Concurence 
- Les données récoltées 

# 06/01/2017: Kalray

[Lien vers les slides](http://chamilo2.grenet.fr/inp/courses/ENSIMAG5MMCTSL0/document/TRANSPARENTS/2016-2017/MPPA_Getting_started_AC27.pdf)

MPPA Getting Start 

Entreprise depuis 2008, 60 personnes, majorité dans la R&D, surtout ingénieurs

MPPA = 28 nm, conso faible -> pour embarqué

2 marchés : 

- Data center très haut débit, traitement du flux réseaux
- Les très bonnes propriétés mémoires du chip, permet de le certifié pour des
  utilisation sécurisées

2 générations de proc:

- Andey : 1 ère version, pas assez bonne gestion des GPIOs
- Bostan : 
	- 80 Gb éthernet
	- PCI
	- 600 MHz de fréquence 
	- FPGA
	- DSP
	- 15 Watt

Plusieurs cartes pour différentes application :

- Embarqué
- Turbo pour calcul intensif
- Une carte de développement 
- ?

Pour utilisé ce matériel, il faut du logiciel:

- Driver
- Library optimisé en asm
- Débug
- Développement

## Architecture du MPPA

- DDR3 2 x 64-bit + ECC
- PCIe 2x8 lane
- 1/10/40 Go d'Ethernet
- GPIO
- NoC

- 16 cluster au centre
	- Chacun de ces 16 cluster a 16 coeurs
	- +1 Système Coeurs qui gère le NoC du cluster, et c'est le seul qui a accès
	  à l'extérieur du coeurs
	- Chaque cluster partage 2 MB de mémoire avec un débit de 77 GB/s
		- Chaque coeurs sont des VLIW 32-bit
		- 5 instructions pas cycle à 600 MHz
		- 8 kB de cache instructoin
		- Instruction totalement propre à Kalray
	- Chaque cluster on 2 NoC : D(ata)-NoC avec DMA + C-NoC with control

- Autour des 16 clusters au centre, 2 cluster de 8 coeurs d'I/O
	- et chaque coeurs sont coupés en deux : donc 2 * 2 * 4 coeurs
	- Il y a donc 16 coeurs d'I/O
	- Sur un cluster : linux
	- Sur l'autre : Bare Metal

- Cloisonement dur des clusters centraux :
	- 4 + 4 + 4 + 4 qui sont vraiment disjoint entre eux
	- Appréciable pour les utlisations critiques

- Le lien PCI permet de rendre le MPPA maitre ou esclave.

## Software

- Environnement C/C++
- Simulateur, profiling
- Driver
- Librairy d'optimisation
- OS : BareMetal, Linux ou intermédiaire 

Outil de développement spécifique : 

- Mode explicite : il faudrait faire des appelles à des fonctions pour apporter
  les données dans le cluster.
	- Exemple : ```frame = load_data(addr, size);```
	- C Low level : Programmation de type DSP
	- C Posix Level : Programmation de type CPU
- Mode implicite : On peut faire (grâce à la MMU) des accès "direct" à la
  mémoire. 
	- Exemple : ```frame = *image;```
	- OpenCL : Programmation de type GPU style
	- OpenDataPlane (ODP), Open API for networking

- Mode explicite : 
	- Démarage des cluster et des I/O explicitement
- Mode inmplicite 
	- Pas besoin de faire des appelles explicites, la MMU s'occupe de tous les
	  accès mémoire

- Outil pour compilation, de débug et de trace système
	- Les outils de traces permet de laisser des points de traces durant
	  l'execution pour une trait faible contre partie.

## Architecture mémoire du MMPA

Chaque coeur à un petit cache de niveau L1. Et tous ces caches ne sont pas
cohérents.
Toutes les coeurs d'un cluster partage aussi une même mémoire partagée  
Attention, la gestion du cache est assez tricky, car les caches sont incohérents

Le DSM permet d'emuler dans le cache des coeurs d'I/O un cache L2 (pour cacher
les pages de la DDR) et avoir toute la DDR adressage.

## Le Marché, voiture autonome

Capable de guarantire en temps réel un débit avec unfaible latence

Moins de 10 fois moins de consomations qu'un CPU 64-bit classique

## Le Marché, du stockage de data

La quantité de données qui arrivent pour être stocker, il faut traiter les
données à la volé pour reduire la quantité de donnée stockée tout en lui donnant
le maximum de valeur.

# 20/01/2017 : Thales
Ingénieurie Logicielle en Developpement Embarqué certifié critique 

Laurent IMPERATRICE
Charles DONNAT

## Thales

62 000 employés, 56 pays, R&D 707 millions
50% civil et 50% militaire
Aéronautique, Espace, Transpor, défense, ...

## Flight avionics

Proposer les différents équipement électrinique des avions.

Centrales inertielles, GPS, Capteurs, instrument de secours

C, C++, ADA 2012

## Un cadre normatif DO178B

Définir la crétissité des pannes, par niveau et essayer de définir des mesures à
prendre lors de la création du code pour que ca soit acceptable.

La DO donne des objectifs à tenir mais ne demande rien quand aux moyens d'y
parvenir

### Processus de développement 

Les objectifs sont : 
- Développer les exigences système, en créant un ou plusieurs niveaux
d’exigences Software (dépendant du niveau de DAL) 
- Développer l’architecture Software
- Produire le Code Source
- Intégrer les composants Logiciel pour produire un Exécutable

Un artefacts logiciels = tous les produits du développement logiciel

Durant le processus de dev, plusieurs artefacts doivent être produit: 

- Exigences de haut et bas niveau (HLR/LLR) 
- Description de l’architecture logicielle  
- Code Source
- Code Object Exécutable

### Processus de tracabilité

Il faut que chaque ligne de code soit justifiée.

Tout élément du binaire doit correspondre directement au code sources et doit
correcpondre à un besoin du produit, ni plus ni moins.

### Processus de vérification

On ne regarde pas le code, mais les exigeances.

Le code doit être couvert à 100% par les tests.

Cette norme est imposé, acceptée par tous les industriels, et audités par des
organismes externes.

Le cycle en V c'est bien mais :

- Trop chère à faire des changements à la fin
- Pas assez flaxible
- Si le client n'est pas content à la fin, aie aie aie
- Gros problèmes entre le SW et le HW
- Démotivation du personelle 

## Les méthodes propres à Thales pour parvenir à respecter ces normes

### Développement itératif incrémental 

Le client veux quelques choses, l'équipe va le faire de façon pas à pas, morceau
par morceau, mais il faudra les rassembler après : c'est l'itératif.

Le client veux quelques choses, l'équipe va commencer par une ébauche et va
l'améliorer de plus en plus: c'est l'incrémental 

Les deux ensemlbe fait de l'itératif incrémental

### SCRUM

Une équipe, c'est : 

- 1 product owner : personne qui connait très bien les spécs du client
- 7 +- 2 personnes, pluri-disciplinaires, co-localisées
- 1 ScumMaster

Il peut y avoire des multi-Scrum:

- Plusieurs équipes
- 1 super ScumMaster pour la synchronisation des équipes
- 1 super Product Owner garant du produit

Itération:

- Pas trop long ni trop court

Product backlog :

- Liste des fonctionnalités du produit
- Fait par le Product Owner
- Prioritées et estimées
- Mise à jour à chaque itération

Stand up Meeting:

- Moment ou l'équipe se réunie pour parler des problèmes rencontrés
- Faire un point sur : ce qui est fait, ce qui va, ce qui block, ce qui va être
  fait

### eXtreme Programming - Développement 

__Problèmatique__ 

- Développer le logiciel avec indépendance
- Transférer les compétences en continue

__Approche__

- Pair programming
	- Sujet complexe
	- Formation
	- Partage de bonnes pratique
- Polyvalence des développeurs
- Non Systématique

### eXtreme Programming - Test

__Problèmatique__ 

- Plusieurs niveau HLR/LLR
- Tester les exigences
- Assurer que les tests soient complets

__Approche__

- Ecrire en même temps les tests et les exigenaces
- Ecrire les tests avants le code
- ...


### eXtreme Programming - Intégration continue

__Problèmatique__ 

- Livrer un logiciel à tout moment

__Approche__

- Integret de façon continue le code des autres 
- Equipe mise à jour de façon continue
	- Feedback plus rapide 
	- Correction rapide
	- Stop the line
- Production de warnings en continue 

__Dificultés__

- Durée des tests automatiques
- Stabilité de l'intégration continue, certain patch peux casser des choses qui
  passaient

__Solutions__

- Optimisation des tests automatique
	- Parallélisation
	- Certain server font tourner des tests toutes la journée
	- Certain server font tourner des tests toutes la nuit
- Rejouer TOUS les tests avant la livraison

### Lean - Value Stream Mapping

__Problèmatique__

Comment se mettre d'accord sur une liste d'acitivité à faire

__Approche__

- Réalisation d'un VSM

### Lean - Kankan (TAsk board)

Un tableau qui représente chaque tache à l'endroit où en est son avancement 

### Lean - Management visuel

__Problèmatique__

Comment rendre les problèmes visibles ?

__Approche__

- Création d'un espace dédié par projet 

### Lean - PDCA 

__Problématique__ 

Comment résoudre les problèmes montrés par toutes ces techniques

__Approche__

- Plan  : Décrire, Chiffrer la situation, Classer les causes racines
- Do    : Liste des problèmes
- Check : Comment on peut vérifier si le problème se résoud ou s'empire
- Act   : Comment réagir


## Conception et développement - Techno objet

Il faut absolument assurer que le code produit soit de qualité, testable,
portable, evolutif, ré-utilisable.

Pour cela, Analyse, Conception, Implémentation

Programmation par contrat :

- Pré-condition
- Post-condition
- Invariant

## Des outils adaptés 

- Pilotage : un projet est fini quand toutes les taches sont faites, non pas
  quand on a passé toutes nos heures sur le projet. (EVM = Earned Value General)




{% include links.html %}
