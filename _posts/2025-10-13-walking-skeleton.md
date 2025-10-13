---
layout: post
title: "Walking Skeleton"
---

## Pourquoi déployer et tester dès la première feature ?

Parce que _spoiler alert_ : ton code ne tourne pas que sur ta machine.

- Ça aide à voir comment ton super système s’intègre dans le monde réel, celui avec des API qui plantent et des firewalls qui disent non.

- Ça met en lumière tous les **risques techniques et organisationnels** que tu vas découvrir _trop tard_ sinon.

- Et surtout, ça te force à comprendre **avec qui tu vas devoir bosser** (les sysadmins, les devops, les gens qui te répondent “ouvre un ticket JIRA”).


Bref, mieux vaut galérer un peu dès le début que pleurer à la fin.

---

## Comment tester la première fonctionnalité ?

1. Tu **automatises la construction, le déploiement et les tests** de cette première feature. Oui, même si “c’est juste un POC”.

2. Ensuite, tu utilises cette jolie infra pour **écrire des tests d’acceptation**, histoire de pouvoir dormir tranquille (ou au moins faire semblant).


---

## C’est quoi un “walking skeleton” ?

C’est pas un zombie de projet, même si ça y ressemble un peu.  
C’est juste une version **squelettique mais fonctionnelle** de ton appli : le strict minimum pour prouver que tout le pipeline marche du code au déploiement.

Concrètement :

- Tu automatises la construction, le déploiement et les tests dans un environnement **iso-prod** (parce qu’en vrai, “ça marche en dev” ne veut rien dire).

- Tu prends **les décisions minimales vitales**, genre quelles technos tu vas maudire pendant 3 ans.

- Tu exposes un peu le **contexte du projet**, histoire que tout le monde comprenne pourquoi ça rame.

- Tu **acceptes que tes premières idées sont probablement nulles**, mais tu y vas quand même.

- Tu définis la **communication minimale entre les composants**, juste pour voir si ça respire.

- Et tu prends le temps de poser une **infrastructure propre**, parce que tu vas la traîner longtemps.


Exemple : pour une **web app avec base de données**, ton squelette, c’est une **page moche** avec quelques **champs qui sortent de la base**. C’est pas sexy, mais c’est la base de ta future usine à gaz.  
Écris les tests que **tu voudrais lire**, pas ceux que tu vas haïr dans 6 mois.

---

## Comment faire un déploiement (et l’automatiser) ?

Faire des **tests de bout en bout**, c’est souvent si galère que tu finis avec une **infra bancale**, des **serveurs factices** et des **promesses de refacto “plus tard”**.  
Mais plus vite tu testes contre un vrai serveur, plus vite tu découvres les vrais problèmes. (Et oui, il y en a.)

Les déclencheurs de ton système doivent être **externalisés**, sinon tu vas passer ta vie à attendre qu’un cron tourne une fois par semaine.  
Un **ordonnanceur externe** qui envoie des messages, ça se remplace, ça se teste, et ça évite les crises de nerfs.

Et surtout, **maintiens un schéma public du système**. Pas parce que c’est beau, mais parce que sinon plus personne ne saura où brancher quoi après deux sprints.

---

## Comment faire du TDD sur un système existant ?

Courage, déjà.  
Ensuite :

- **Automatise tout** : build, déploiement, tests. Parce que tu vas recommencer souvent.

- **Ajoute des tests de bout en bout** sur le chemin le plus simple, histoire de commencer quelque part.

- Et glisse des **tests unitaires** petit à petit en ajoutant des nouvelles features.  
  C’est un marathon, pas un commit.