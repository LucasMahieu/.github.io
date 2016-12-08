---
title: Note de Cours
sidebar: mydoc_sidebar
permalink: cours_sse.html
folder: mydoc/sse
---

# Cours du 10/10/16

## Nécessité de "dépacker" la puce
* Carte à puce
	- Sous les 6 plots d'une carte à puce se trouve la puce
	- Une fois que le circuit (puce) est sortie, il y a que 6 broches moins 2 (VCC, GND), ce qui fait 4.
	- Peu compliqué 
* Carte RFID
	- Traitement chimique pour supprimer le plastique et récuperer la puce RFID

Une fois que le circuit est "nu" on peut l'exploiter pour les attaques.
	
{% include important.html content="Le fait d'enlever le package, le circuit sera exposé à la lumière.<br/>
Les concepteurs ont pu mettre des capteurs de lumière ou d'UV pour que le circit se bloque en cas d'exposition à la lumière" %}

## Observation du circuit
Avant, on pouvait observer directement le circuit et comprendre son fonctionement.
Or mtn, le circuit est fait de bcp de couches qui sont homogénéisés pour ne plus pouvoir récuperer d'information par observation.
Le métal ajouté s'appelle les : "Dummies"

## Enlèvement des couches
Par des traitements chimiques, on peut enlever les courches de métal, d'isolant etc ...
Il est ensuite possible d'obeserver avec des microscopes assez puissant les differents éléments qui composent le circuit
Par cette méthode on peut :   
	- Repérer l'emplacement des composants   
	- Recuperer des données stockés en dure dans des ROM   
	
Pour rendre la tâche plus difficile à l'attaquant, on peut stocker les octets en ROM dans des ordres non convencionel : Trumbling
Mais si on faire la même observation sur le décodeur de ROM, on peut encore une fois retrouver les données de la ROM

## Autres interventions physiques

- Couper des interconnexions internes
- Ajouter ou modifier des interconnexions internes
Pas toujours facile sans abimer les autres zones du circuit
Mais l'une des utilisations pourrait être de détruire ou reconstruire des fusibles de sécurité.

## Observation dynamique du circuit
Avec des SEM, Microscopes électroniques, on peut observer les niveaus logique dans un circuit au runtime.
Très utilisé pour aller lire des mémoires programmables qui pourraient contenir des clés secrétes.

## Attaque par canaux caché/auxiliaire

- Mesure du temps d'exécution
- Mesure de consommation de courant (SPA, DPA, CPA)
- Mesure des émissions EM
- Mesure de son, chaleurs, photons
- Ou d'autres encore ... ?

## Quelle conditions pour faire de telles attaques ?

- Il dois y avoir un lien entre la clé secrète et une grandeur physique
- Et pour savoir quelle grandeur doit être mesuré, il faut connaitre quel algo est utilisé

## Type d'attaque par canaux auxiliaire

- Simple 
	- Grandeur directement mesurable 
- Différentiel
	- Nécecite plusieurs mesures, qui peut être très important

## Les attaques par cannaux auxiliaire

### Attaque par analyse de temps

* Dans des boucles If Then Else il peut y avoir des temps d'exécution différents en fonction des valeurs d'entrée
* Il faut des fois accepter de perdre en performance pour plus de sécurité :
	* Implém 1: if(x=1) then a=a+b;
	* Implém 2: t[0]=a ; t[1]=a+b ; a=t[x] ;
	Implémentation 2 est sécurisé vis à vis de la mesure de temps.   
	En contre partie, plus d'accés mémoire
* Autre exemple : code PIN de la carte bleu 
* Même si la mesure ne permet pas de dévoiler la clé, ces méthodes de mesure de temps peut permettre de construire des statistiques qui permettrons de réduire considérablement le nombre de possibilité à essayer par bute-force 

### Attaque par analyse de consommation

* SPA : Simple Power Analysis
	* Un transistor consomme du courant lorsqu'il commute  
	* En mesurant le courant, on peut remonter aux "bits" du système  
	* On repère un motif qui revèle la clé, vu qu'on connais les algo utilisé  
* DPA : Differencial Power Analysis
	* Il faut des modèles pour lier la physique aux inforamtions qui se propagent dans le système  
	* Il faut ensuite trouver l'opération qui nous permettra de trouver l'information voulu  
	* Par exemple M' = f( M XOR K )
	* On découpe ce genre d'opération en sous bloc, on fait des hypothèses de clés et voir l'influence sur la conso de courant

### Attaque par analyse d'émission électronmagnetique

Méthode plus complexe car dépend beaucoup du matériel et de son utilisation  
Mais les courbes obtenues sont bien plus précise car aucun "filtre".   
En effet, lors de la mesure de courant, les capacités du systèmes filtre le courant mesuré.  
Ce n'est pas le cas des mesures EM.

### Attaque par analyse d'émission lumineuse

Permet de visualiser l'état de certaines informations transitant dans le circuit.

## Attaque par perturbation du circuit

* Modification des conditions de fonctionnement
	* Température
	* Tension d'alimentation
	* Fréquence d'horloge
	* ...  
	Ces attaques peuvent être protégé par utilisation de capteurs
* Sans modification des conditions et en injectant des erreurs dans le systèmes
	* Glitches (power and clock)  
	* Injection de particules : UV, etc 
	Ici aussi il peut y avoir des capteurs pour protéger

__Pourquoi injecter des fautes ?__  
Exemple de l'attaque de Bellecore sur RSA CRT  
Avec un flash on perturbe un chiffrement et avec quelques propriétés mathématiques, on peut retrouver la clé très facilement.

__Une attaque idéale serait__  

* Localisée (x,y)
* Localisé en temps (début et durée)
* Type de la faute : stuck at, injection d'erreurs
* Bas cout
* Rapide


# Cours du 17/10/16

## Création d'erreur   
SET: Single-Event Transients   
SEU: Single-Event Upset  
MBU:   

Attaque software:   
	- SW flaw exploitation, test protocols  
	- Buffer overflow  
	- Trojan horses (e.g., hidden in games,...)  
	- Virus, code agressif, attaque par réseaux  

### Trojans (cheveau de troi)

* HW spys / wireless reporting
* Hidden "bombs" (passage à l'an 2000)
* PUF = Physically Unclonable Functions:
	* Puces qui permet d'identifier un objet
	* Il faut faire un circuit, et le remplir aléatoirement, qu'on ne contrôle pas
	* Un hacker ne pourra que reproduire le circuit et pas l'aléatoire
	* Ce nombre doit être reproductible mais unique (différent des autres produit)
	* Ex: Initialisation d'un plan mémoire, ...

## Problématiques des tests
Il y a un vrai problème avec le test de circuits critiques, les moyens de test pourrais etre une énorme opportunité pour un attaquant.     
Par exemple, la chaine de scan peut faire sortir des clés de chiffrement   
Il faut donc mettre ne place des mesures de test sans que ça influe la sécurité   

## Pourquoi étudier les modèle de fautes
Radiations de particules qui peuvent faire des fautes par exemple   
Le choix du modèle est important : Bit-flip, Bit-set, Bit-reset   
Mais en pratique, c'est pas tjs facile de faire certaine attaque.   

# Cours du 07/11/16
Contre-mesure  

## Analyse pas canaux auxilières

Attaque liées à certaines grandeur physique 

- Time 
- Power
- EM
- ...

Le principe de ces attaques est de trouver un lien entre ces grandeurs physique
et les données sensibles.  
Les contres mesures consiste donc à rendre le lien bcp plus difficile à trouver.  

SPA = Simple Power Analysis -> pas très utile pour les algo symétrique  
DPA = Differential Power Analysis  

Il faut donc rendre complexe la mesure de ces grandeurs physiques  
Sécurisation de 'scan chain' qui donne accès au circuit  

## Structure de tests 

Scambling méthode :  
Faire des scan chaine non linéaire, mais aléatoire par segment   

Scan-Enable Tree:  
Vérifier l'intégrité du signal

Spy Flip-Flop:  
tester si le contrôleur de test à un comportement "attendu", "correcte" 

Built in Self Test (BIST):  
Architecture en 3 'blocs' : 

- TPG: Génère des vecteurs de tests
- DUT: Devise Under Test 
- ORACLE: Comparateur de sortie 

## Attack par faute

### AES c'est quoi ? 

Algo symétrique = même clé pour chiffrer et déchiffrer  
Text = 128b  
Clé = 128/192/256b
Tour pour un chiffrement = 10/12/14 rounds  
1 round =   
	- SubBytes: substitution non linéaire de byte  
	- ShiftRows: rotation des lignes  
	- MixColumns: multiplication linéaire par colonne  
	- AddKey : ajout de la clé modifié  

## Code correction/détection d'erreurs
EDC

# Cours 15/11/2016

[lien des
slides](http://chamilo2.grenet.fr/inp/courses/ENSIMAG5MMSSE/document/Transparents_SSE_16-17.pdf?cidReq=ENSIMAG5MMSSE&id_session=0&gidReq=0&origin=)

# Cours 07/12/2016

Cours/TD

__Exemple 1__

Full Adder 

TMR : Triple Modular Redudency 

On triple tout, on ajoute un voteur et on met ca dans une boite 
Attention, à la synthèse il va y avoir une optimisation de tout se qui est
redondant. Donc il faut ajouter des contraintes à l'outil de synthèse.

Un moyen de vérifier peut être de comparer la surface du circuit avec et sans
redondance.

On peut aussi comparer le chemin critique. Celui du TMR devrait être un petit
peu plus grand à cause du voteur.

Dans un flow FPGA il peut y avoir aussi des contraintes de mappings. Si deux
fonctions redondante sont mappés dans la même LUT, dans le même CLB, la
redondance perd tout sont sens.

Il peut aussi y avoir un deuxième problème, si au routage on à les fonctions
redondantes sur des LUT qui utilisent les même "switch box" entre les LUT, la
redondance perdent leur sens.

Il faut donc que chaque fonction de redondance soit dans les LUT différentes et
indépendantes.






{% include links.html %}
