layout: post
title: "JavaFX Installation"
date: 2025-09-29 21:43:00 -0000
categories: CATEGORY-1 CATEGORY-2

## 1. Installation du JDK

Quand tu crées un projet **JavaFX** dans IntelliJ IDEA, IntelliJ télécharge et installe automatiquement un **JDK** dans : `/Library/Java/JavaVirtualMachines/ms-21.0.6/Contents/Home`. Pas besoin d’installer manuellement un JDK. Pas besoin de créer une variable `JAVA_HOME`. IntelliJ gère tout seul, toi tu touches à rien. Si tu t’acharnes à définir `JAVA_HOME` manuellement, c’est que t’as du temps à perdre et pas assez de séries Netflix à mater.

## 2. Pourquoi `java -version` fonctionne sans `JAVA_HOME` ?

Sur macOS, quand tu tapes `java` dans le terminal, t’exécutes pas Java. T’exécutes le **stub Apple** situé dans `/usr/bin/java`. Ce stub appelle `/usr/libexec/java_home` pour localiser le **JDK par défaut**. Son rôle est de **parcourir les dossiers où les JDK sont installés** et de choisir lequel doit être considéré comme “par défaut”. `~/Library/Java/JavaVirtualMachines/` est un emplacement typique. Et là seulement, le vrai binaire débarque.

- `/usr/bin` contient les jouets préinstallés par Apple.
- `/usr/local/bin` contient des programmes installés par l’utilisateur (ex: via Homebrew).

## 3. Quand définir `JAVA_HOME` ?

**Utile pour** : Maven, Gradle, Ant, Tomcat ont souvent besoin de la variable. Ces trucs sont comme des bébés : si tu leur donnes pas leur `JAVA_HOME`, ils se mettent à hurler en rouge dans ta console.
**Indispensable** si tu jongles avec plusieurs JDK (ex: 17 et 21) pour forcer un outil à utiliser une version donnée.
**Jamais** si tu bosses tranquille dans IntelliJ. Sérieusement, IntelliJ s’en fout. Si tu définis `JAVA_HOME` “au cas où”, c’est comme mettre deux capotes : inutile et ridicule.

## 4. Maven dans IntelliJ

IntelliJ **embarque son propre exécutable Maven** par défaut (“bundled”). Tu peux le vérifier dans `Preferences → Build, Execution, Deployment → Build Tools → Maven` → Champ _Maven home directory_.

Deux options possibles :
1. **Bundled (Maven 3.x)** → celui qui est intégré à IntelliJ. Bundeled, c’est juste du jargon tech qui signifie : **inclus avec le logiciel**. Tu veux la preuve ? Tape `mvn -version` dans ton terminal → “command not found”.  
   Voilà. Pas de Maven installé.
2. **Maven externe** → si tu l’as installé toi-même.

Il se fiche de `JAVA_HOME`.  Il utilise le **JDK configuré dans IntelliJ** (celui que tu as choisi dans **Project SDK** ou **Build Tools → Maven → JDK**).

## 5. Git et IntelliJ

Quand tu coches **“Create Git repository”**, IntelliJ initialise un dépôt (`.git/`) dans ton projet et prépare l’interface pour gérer les commits, branches, etc. mais pour que ça fonctionne vraiment (commits, push, pull…), **Git doit être installé sur ton Mac**.
macOS pré-installe Git avec les outils de développement **Xcode Command Line Tools**.
IntelliJ le trouve grâce au **PATH**, le `PATH` est une **variable d’environnement** qui contient une liste de **dossiers** où le système va chercher les exécutables quand tu tapes une commande dans le terminal.

## 6. GroupId et ArtifactId (Maven)

Quand tu configures ton projet Maven, IntelliJ demande :
### 🔹 Le GroupId

Ton blaze officiel. Identifie ton organisation / auteur du projet.
Convention : **nom de domaine inversé**.
Ça permet d’éviter les collisions de noms si deux projets ont le même artifact.
Exemples :
- `org.springframework`
- `com.google.guava`
- `fr.monsite.projets`

Pour un projet perso : `com.nomprenom` ou `fr.monprojet`.

### 🔹 ArtifactId

- Nom unique de ton projet ou du module au sein du group
- Doit être en **minuscules**, descriptif et sans espaces.
- On peut utiliser `-` pour séparer les mots.
- Exemples :

- `gestion-stock`
- `calculatrice`
- `site-personnel`
### 🔹 Version

La **version** est souvent demandée :

Commence par `1.0.0`.  
Ensuite :

- `1.0.1` → tu corriges une faute de frappe.
- `1.1.0` → tu rajoutes une feature pété.
- `2.0.0` → t’as tout cassé, tu fais semblant que c’est une “refonte”.

### ✅ Exemple complet

Projet : gestion de notes scolaires.

```xml
<groupId>com.alicedupont</groupId>
<artifactId>gestion-notes</artifactId>
<version>1.0.0</version>
```

Ce projet serait publié comme :

```
com.alicedupont:gestion-notes:1.0.0
```

Et voilà, t’as coché trois cases et Maven te fait croire que t’es un architecte logiciel.

![[Pasted image 20250904215443.png]]

https://www.jetbrains.com/help/idea/javafx.html#package-app-with-jlink