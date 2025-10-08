---
layout: post
title: "Objects communicates"
---

## Comment concevoir des systèmes puissants (sans te tirer une balle dans le pied)

Tu veux construire un système qui tienne la route ?  
Pas juste un truc qui “marche sur ma machine” ?  
Alors arrête de penser **objets, classes, héritage** et compagnie.  
Concentre-toi sur **comment tes objets se parlent**.

Parce qu’au fond, ce qui fait tourner ton appli,  
c’est pas la hiérarchie UML en forme de sapin de Noël,  
c’est les **interactions** entre tes objets.  
C’est leur **conversation**, pas leur ADN.

---

## Ok, mais… comment un objet “parle” exactement ?

Simple :

- il **reçoit des messages** d’autres objets,

- il **réagit** (en envoyant à son tour des messages),

- il **renvoie une valeur ou une exception**,

- et il garde pour lui un **état interne** (histoire de pas oublier ce qu’il a fait).


En gros : ton objet, c’est un petit être social, pas un moine bouddhiste.  
Il a besoin des autres pour bosser.

---

## Ce qu’on veut, c’est **ce que le système fait**, pas **comment il le fait**

Pour y arriver, tes objets doivent :

- être **connectables**,

- avoir des **dépendances explicites**,

- jouer un **rôle clair** via une interface,

- et **savoir quand envoyer un message**.


C’est ça, la base. Pas 300 classes héritées d’un `AbstractSomethingFactory`.

---

## Valeurs vs Objets : la guerre du siècle

Les **valeurs**, c’est des chiffres, des dates, des points.  
Elles bougent pas, elles sont **immuables**.  
Deux valeurs identiques ? → même chose.

Les **objets**, eux, c’est une autre histoire :

- ils ont une **identité** (comme toi, même après 3 cafés),

- leur état **change dans le temps**,

- ils **représentent des processus**, pas juste des données,

- et même s’ils ont le même état à un instant T,  
  ils peuvent diverger dès qu’un message différent leur arrive.


💡 En clair : une **valeur** dit “je suis ça”,  
un **objet** dit “je fais ça”.

---

## Rôles, responsabilités et collaborations

- Un **objet**, c’est une **implémentation de rôles**.

- Un **rôle**, c’est un **ensemble de responsabilités**.

- Une **responsabilité**, c’est une **obligation** : faire un truc ou connaître une info.

- Une **collaboration**, c’est quand ces rôles bossent ensemble.


Et pour t’y retrouver, y’a les fameuses **cartes CRC** :  
_Candidates, Responsibilities, Collaborators._  
(Oui, c’est un peu old-school, mais diablement efficace.)

---

## La loi de Déméter — ou “Tell, don’t ask”

Arrête de fouiller dans les tripes de tes objets.  
Dis-leur **ce que tu veux**, pas **comment le faire**.

Ton code :

`((EditSaveCustomizer) master.getModelisable()   	.getDockablePanel()         .getCustomizer())    	.getSaveItem()        .setEnabled(Boolean.FALSE.booleanValue());`

C’est un cauchemar.  
Fais-toi (et ton équipe) une faveur :

`master.allowSavingOfCustomisations();`

Bam. Lisible. Clair. Tu sais ce que ça fait.  
Pas besoin de Sherlock Holmes pour deviner.

---

## Quand poser des questions à un objet ?

Seulement si c’est :

- pour **lire une valeur** (ou une collection),

- pour **créer un nouveau machin** via une **factory**,

- ou pour **poser une vraie question** (pas pour bidouiller son état interne).


Regarde ça :

`public void reserveSeats(ReservationRequest request) { 	for (Carriage carriage : carriages) { 		if (carriage.getSeats().getPercentReserved() < percentReservedBarrier) { 			request.reserveSeatsIn(carriage); 			return; 		} 	} 	request.cannotFindSeats(); }`

Tu peux le dire plus simplement :

`public void reserveSeats(ReservationRequest request) { 	for (Carriage carriage : carriages) { 		if (carriage.hasSeatsAvailableWithin(percentReservedBarrier)) { 			request.reserveSeatsIn(carriage); 			return; 		} 	} 	request.cannotFindSeats(); }`

Lisible. Naturel. On comprend ce que ça fait **sans** scanner toute la classe.

---

## Et pour tester tout ça ?

Tu veux tester un objet sans le disséquer ?  
Pas de souci :

- **Mocke** ses voisins,

- **crée** l’objet cible,

- **définis les attentes** (ce qu’il doit appeler),

- **lance** la méthode,

- et **vérifie** que tout s’est passé comme prévu.


L’idée, c’est pas de “voir ce qu’il a fait dedans”,  
mais de vérifier **qu’il s’est bien comporté** vis-à-vis des autres.

💡 Un bon test, c’est comme une bonne spec :  
on sait **ce qu’on teste**, **comment on le teste**,  
et **quels objets sont impliqués**.

---

Tu veux un système puissant, propre et évolutif ?  
Fais parler tes objets.  
Fais-leur jouer leur rôle.  
Et arrête de leur demander des trucs qu’ils ne devraient pas te dire.