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

* Différence avec Système Embarqués/Centralisés et les Système distribués :   
	* Communication entre processus
	* Mémoire partagée 
	* Absence d'horloge partagée
	* Tolérance au faute
Comment faire pour utiliser ces systèmes ?    

* Spécification :  
	* Communications ? 
	* Synchronisation ?
	* Consensus ?  
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

# Cours du 19/10/2016

## Diffusion de messages

### Best Exeffort Broadcast  (BEB)
- Spécifications informelle: 
	- N machines
	- Si un message est émis par une machine correcte, il doit être délivré (reçu) par toutes les machines correctes.
- Spécification formelle:
	- Interfaces:
		- _in_: broadcast(m): permet de diffuser le message m
		- _out_: deliver(m,p): permet de délivrer un meassage m émis par le processus p
	- Propriétés: 
		- _Validité_: Si un message est émis par une machine correcte, il doit être délivré (reçu) par toutes les machines correctes.
		- _Intégrité_: Idem que le canal fiable.
	- Hypothèses:
		- Panne franche (pas de bornes sur le nombre de fautes)
		- Canaux fiables
	- Algo:

```c
    bf_broadcast(m){
        for(p=0; p<pmax; p++){
            cf_send(m,p);
        }
    }
    cf_deliver(m,p){
        be_deliver(m,p);
    }
```

Rmq:   

1. Tous les processus correcte ne délivrerons pas forcement les mêmes message (ex: msg émis par machine fautive)   
2. Un processus fautif peut délivrer des messages qui ne seront délivrés par aucun processus correct (ex: message émis par machine fautives)   

### Reliable Broadcast (diffusion fiable)
Comment régler le point 1 précédent ? = Comment faire pour que tous les proc. correctes délivre les même msg ?

- Spéc = idem
- Propriétés: 
	- _Accord_: Si un processus correct émet un message m alors il le délivrera
	- _Validité_: idem
	- _Intégrité_: idem
- Hypothèse :
	- Panne franches (pas de bornes)
	- Canaux fiables
- Algo : 
	- Principe : Après avoir délivré le msg,le processus be_broadcast le msg.
	- ATTENTION :
		1. Il faut s'arreter un jour
		2. Intégrité -> Il ne faut délivrer chaque message qu'une fois au plus

=> MEMOIRE INFINIE

# Cours du 24/10/2016

## Diffusion de message : 1 vers n  

Pour garantir la propriété d'accord de RB, on doit multiplier le nombre d'envoi de messages par N, 
donc on divise par N la bande passante de notre canal.

De plus, pour savoir que l'on a déjà retransmis il faut mémoriser cette information.   
Plusieurs possibilité : 

1. On enregistre tous les messages => problème de MÉMOIRE INFINIE   
2. On numérote les messages par sources, et on les délivres par ordre croissant : coût mémoire = O(1)  => pas vraiment efficace
3. On s'autorise une fenêtre de mémorisation => efficace 

## BR plus efficace  

Pour corriger le problème de bande passante de BR on doit trouver une solution  
Une solution est de faire l'hypothèse que l'on a un détecteur de fautes parfait.  
Ainsi, si l'on avait reçu un message de la part d'un processus qui est tombé en panne, on le re-broadcast  

Attention, ici encore problème de MEMOIRE INFINIE, pour se souvenir des messages que l'on devra renvoyer en cas de panne.

Du coup, ici aussi on doit trouver une astuce :
-  Il faut vider la mémoire régulièrement 
- Régulièrement, chaque processus informe les autres de façon "efficace" -> du type = j'ai reçu les 1000 msg de 3000 à 3999.
- __ATTENTION__ que faire en cas d'absence de nouvelles d'un processus
- Pour cela, c'est le détecteur de faute parfait qui nous garanti que l'on va recevoir un message de tous les vivants.  

Il reste encore un comportement non désirable :

- "Les processus fautifs peuvent délivrer des messages qui ne seront pas délivrés par les processus corrects
- Problème d'effets de bords
- Problème du redémarrage de machine 

## URB = Uniform Reliable Broadcast

- Spéc = idem
- Propriétés: 
	- _Validité_: idem RB
	- _Intégrité_: idem RB
	- _Accord Uniforme_: Si un processus (TOUS) élivre un message m, alors tous les processus CORRECT délivrerons m 

- L'idée pour cela est que la machine qui émet un message 
(qui peux tomber en panne après l'avoir délivré) 
doit attendre de recevoir tous les ACK pour le message m avant de le délivrer.
- Puis il notifie tout le monde que le message m est 'stable' = que tout le monde l'a ack  
- Variante: les processus qui reçoivent m envoient un ack à tous les processus, et quant à leur tous ils reçoivent l'ack de m, ils le délivrent.

# Cours du 9/11/16

Tous ces algos ne respectent pas l'ordre d'envoi   
Plusieurs solutions:

- FIFO : Tous les messages émis par un processus p sont délivrés dans l'ordre
  d'émission.
- CAUSAL : Pas dans ce cours, CAUSAL > FIFO
- TOTAL : tous les messages sont délivrés dans le même ordre pour tous les
  processus. Tous les BD exécutent les mêmes requêtes dans le même ordre. Mais
pas forcement dans l'ordre FIFO.

## TOB = Total Order Broadcast 

- Spéc = idem BEB, RB, URB
- Propriété : validité, intégrité, accord => idem RB ou URB
- + ORDRE TOTAL = si un proc p délivre m1 avant m2, alors aucun processus ne
    pourra délivrer m2 avant d'avoir délivré m1.
- Algo : Panne franche + Canal fiable + Détecteur de fautes + séquenceur.
	- Un séquenceur est un processus élu par P.

Quand utiliser le détecteur de faute (P)?  
On l'utilise quand il faut attendre l'ack des autres machines.
Sinon on pourrait attendre un ack infiniment.  

Mais supposer avoir un VRAI P, n'est pas tjs acceptable 

## Paxos = algo de concensus

Consensus      =      TOB  
(Paxos dans P)       (avec P)


- Spéc : Interface :
	- Propose : permet de proposer une valeur
	- Décide : indique la valeur décidée
- Propriétés :
	- Toutes les machines correctes décident la même valeur.
	- La valeur décidé a été proposée par une machine

Paxos = implante la spec du concenssus en faisant les hypothèses suivantes:

- Canale fiable
- Pannes : "crash recovery"

Paxos à besoin de 2f+1 machine pour tolérer f fautes  
=> Suppose f=1 : -> Paxos a besoin de 3 machines  

- Si 0 panne : consensus faisable 
- Si 1 pannes : consensus faisable 
- Si >1 pannes : blocage MAIS PAS D'ERREUR ! BLOCAGE SEULEMENT


Paxos se décompose en 4 régles :

Pour f=1, Av = Accepted value, Sn = Séquence number

1. Si Sn PREPARE > Sn stocké en local -> OK + Av sinon KO
2. Si f+1 OK alors le proposeur peut émettre (ACCEPT, Sn, v)  
	avec Sn = le num utilisé dans le proposeur value  
	v = si f+1 (-1) alors v= ce que veut proposer le proposeur  
		sinon la valeur associé avec le plus grand Sn recu
3. Si Sn du ACCEPT >= Sn stocké alors Av(Sn,v du accept) et OK (sinon KO)

(lien de la video)[https://www.youtube.com/watch?v=cj9DCYac3dw]

# Cours du 16/11/16

## Notion de performance 

Exemple : Protocole de performance basique  

Métrique : 

	- Débit : nombre de message déivré par unité de temps => système doit être
	  saturé 
	- Latence : temps entre l'émission et la délivrance par le "dernier" proc.
	  => le système ne doit pas être saturé 
	- Temps de réponse :

Système physique sur lequel s'exerce le protocole :  
Cas d'étude : Datacenter  : simple et réaliste

Modèle de rondes : chaque processus peut :

- émettre 1 message AU PLUS au début de chaque ronde.
- recevoir 1 message AU PLUS à la fin de chaque ronde.

Pourquoi modéliser : 

1. Avoir une approximation des performances sans implémenter le système   
2. Résonner sur latence vs. débit : débit optimal, latence optimal ...   

Pour optimiser la latence :  
p1 envoie m à p2 au ronde 1  
p1 envoie m à p3 et p2 envoie m à p4 au ronde 2  

Latence = 2, Debit = 0.5 m / ronde

Pour un débit optimal :   
p1 envoie m à p2
p2 envoie m à p3 et p1 envoie m2 à p2
p3 envoie m à p4 et p2 envoie m2 à p2 et p1 envoie m3 à p2
p3 envoie m2 à p4 et p2 envoie m3 à p3
p3 envoie m3 à p4

Latence = 3 rondes, Débit = 1 msg / ronde

Le débit maximal entre N machine est = N/(N-1)


{% include links.html %}
