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

## Type d'attaque par cannaux auxiliaire
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
	Un transistor consomme du courant lorsqu'il commute  
	En mesurant le courant, on peut remonter aux "bits" du système  
	On repère un motif qui revèle la clé, vu qu'on connais les algo utilisé  
* DPA : Differencial Power Analysis
	- Il faut des modèles pour lier la physique aux inforamtions qui se propagent dans le système  
	- Il faut ensuite trouver l'opération qui nous permettra de trouver l'information voulu  
	- Par exemple M' = f( M XOR K )
	- On découpe ce genre d'opération en sous bloc, on fait des hypothèses de clés et voir l'influence sur la conso de courant

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

__Une attauque idéale serait__  
* Localisée (x,y)
* Localisé en temps (début et durée)
* Type de la faute : stuck at, injection d'erreurs
* Bas cout
* Rapide


# Cours du 17/10/16



{% include links.html %}
