---
title: "Chapitre 5 - Commandes de Workflow Git partie 1"
description: "Découvrez les commandes de workflow Git pour gérer un projet localement et le synchroniser avec un dépôt distant comme GitHub"
emoji: "🔧"
---

---
<a name="table-des-matieres"></a>

## Table des matières
---

1. [Introduction](#introduction)
2. [Dépôt Local](#depot-local)
3. [Dépôt Distant](#depot-distant)
4. [Synchronisation avec GitHub](#synchronisation-avec-github)
5. [Connexion à GitHub via des clés SSH](#connexion-a-github-via-des-cles-ssh)
6. [Conclusion](#conclusion)


<br/>
---

<a name="introduction"></a>
# Introduction
---

Les **commandes de workflow Git** permettent de gérer un projet localement et de le synchroniser avec un dépôt distant comme GitHub. Ce guide vous montrera les différentes étapes pour gérer un dépôt Git local, effectuer des commits, et pousser le code vers GitHub.

#### [⬆️ Retour à la table des matières](#table-des-matieres)
<br/>

---

<a name="depot-local"></a>
## 1 - Dépôt Local : `Git`
---
Un **dépôt local** est comme un dossier spécial sur votre ordinateur qui garde une trace de tous les changements que vous faites dans vos fichiers. Imaginez-le comme un album photo personnel où vous conservez toutes les versions de vos photos - chaque modification est enregistrée et vous pouvez revenir en arrière si nécessaire.

#### [⬆️ Retour à la table des matières](#table-des-matieres)
<br/>

---

### 1.1 - Caractéristiques du dépôt local :
- Il est stocké sur **votre ordinateur**
- Vous pouvez travailler **sans connexion internet**
- C'est votre "espace de travail personnel" 
- Seul vous y avez accès

Un **dépôt distant** (comme GitHub), c'est comme un album photo en ligne que vous pouvez partager avec d'autres personnes. C'est une copie de votre projet qui est stockée sur Internet.

#### [⬆️ Retour à la table des matières](#table-des-matieres)
<br/>

---

<a name="caracteristiques-du-depot-distant"></a>
### 1.2 - Caractéristiques du dépôt distant :
- Il est hébergé sur **Internet** (ex: GitHub)
- Nécessite une **connexion Internet** pour y accéder
- Permet de **partager** votre code avec d'autres
- Sert de **sauvegarde** si votre ordinateur tombe en panne

### Analogie simple :
Imaginez que vous êtes un photographe :
- Le **dépôt local** est comme votre appareil photo où vous prenez et retouchez vos photos
- Le **dépôt distant** est comme une galerie d'art où vous exposez vos meilleures photos sur instagram ou facebook.
- **Git** est comme votre assistant qui :
  - Note chaque modification que vous faites
  - Garde une trace de quand et pourquoi vous avez fait ces changements
  - Vous permet de revenir à une version précédente si nécessaire
  - Vous aide à fusionner votre travail avec celui d'autres photographes

Git joue donc le rôle crucial d'un gestionnaire de versions qui :
- Suit l'évolution de votre projet
- Protège votre travail contre les erreurs
- Facilite la collaboration avec d'autres
- Permet de gérer plusieurs versions en parallèle

Cette organisation vous permet de travailler sereinement sur votre ordinateur et de ne partager votre travail que lorsqu'il est prêt !



#### [⬆️ Retour à la table des matières](#table-des-matieres)
<br/>

---

<a name="depot-distant"></a>
## 2 - Dépôt Distant : `GitHub` ([https://github.com](https://github.com))
---

Un dépôt distant, c'est comme un "coffre-fort en ligne" pour votre code ! Il existe plusieurs services populaires qui proposent d'héberger vos dépôts :

- **GitHub** : Le plus connu, comme un "Facebook du code". Idéal pour les projets open source.
- **Azure DevOps** (Microsoft) : Parfait pour les entreprises qui utilisent déjà les outils Microsoft. Propose aussi des outils de gestion de projet.
- **AWS CodeCommit** : La solution d'Amazon, bien intégrée avec les autres services AWS.
- **Bitbucket** (Atlassian) : Très apprécié des équipes qui utilisent déjà Jira ou Confluence.
- **GitLab** : Une alternative complète qui peut être hébergée sur vos propres serveurs.

Ces dépôts distants vous permettent de :
- Sauvegarder votre code (comme une "copie de secours en ligne")
- Collaborer avec d'autres développeurs (comme un "Google Docs du code")
- Suivre qui a fait quoi et quand (comme un "journal de bord")
- Gérer différentes versions de votre projet (comme des "points de sauvegarde")

C'est un peu comme si vous aviez un super assistant qui garde une copie de tout votre travail en lieu sûr et qui vous aide à travailler en équipe !


#### [⬆️ Retour à la table des matières](#table-des-matieres)
<br/>

---

<a name="etapes-du-workflow-git"></a>

## 3 - Étapes du workflow Git

### 1 - Créer un dossier pour le projet
Créez un dossier pour contenir votre projet.
```bash
mkdir SpringBootMicroservice
```

### 2 - Créer les fichiers nécessaires
Naviguez dans le dossier et créez quelques fichiers Java.
```bash
cd SpringBootMicroservice
touch restapi.java dao.java services.java
```

### 3 - Initialiser le dépôt Git
Initialisez le dépôt Git dans le dossier du projet.
```bash
git init
```

### 4 - Vérifier la création du répertoire `.git`
Après l'initialisation, un dossier `.git` est créé. Ce dossier est responsable de la gestion des versions du projet.
```bash
ls -la
ls .git
```

### 5 - Configurer le nom de l'auteur et l'adresse email au niveau local
Configurez le nom et l'email de l'auteur pour ce projet localement.
```bash
git config --local user.name "Haythem"
git config --local user.email "rehoumahaythem@gmail.com"
```

### 6 - Vérifier la configuration locale
Vérifiez que le nom et l'email sont correctement configurés dans le fichier `.git/config`.
```bash
cat .git/config
```

### 7 - Vérifier l'état du dépôt local
Vérifiez l'état du dépôt pour voir quels fichiers ne sont pas suivis (fichiers non trackés).
```bash
git status
```

### 8 - Ajouter un fichier dans la zone de staging
Ajoutez un fichier spécifique (par exemple, `dao.java`) à la zone de staging.
```bash
git add dao.java
git status
```

### 9 - Retirer un fichier de la zone de staging
Si vous souhaitez retirer un fichier de la zone de staging (le rendre non tracké), utilisez la commande suivante :
```bash
git rm --cached dao.java
git status
```

### 10 - Ajouter tous les fichiers à la zone de staging
Ajoutez tous les fichiers à la zone de staging en une seule commande.
```bash
git add .   # ou git add -A
git status
```

### 11 - Commiter un fichier spécifique dans le dépôt local
Commitez un fichier spécifique (par exemple, `dao.java`) dans le dépôt local.
```bash
git commit -m "dao.java est ajouté"
git log
```

### 12 - Commiter tous les fichiers dans le dépôt local
Commitez tous les fichiers dans le dépôt local.
```bash
git commit -m "Ajout des fichiers restapi.java et services.java"
git status
git log
git log --oneline
```

#### [⬆️ Retour à la table des matières](#table-des-matieres)
<br/>

---

<a name="synchronisation-avec-github"></a>
## 4 - Synchronisation avec GitHub

### 1 - Créer un dépôt sur GitHub
1. Rendez-vous sur [GitHub](https://github.com) et connectez-vous.
2. Cliquez sur **"New"** ou **"Start a project"** pour créer un nouveau dépôt.
3. Donnez un nom à votre dépôt (par exemple **SpringBootMicroservice**), sélectionnez l'option **Public**, puis cliquez sur **"Create Repository"**.

### 2 - Créer un token GitHub
1. Allez dans les **Settings** de votre compte GitHub.
2. Sélectionnez **Developer Settings**, puis **Personal Access Tokens**.
3. Cliquez sur **Generate new token** et remplissez les informations :
   - **Note** : `token1`
   - **Expiration** : 7 jours
   - **Permissions** : cochez les options **repo** et **admin:org**.
4. Cliquez sur **Generate token** et notez le numéro du token.

### 3 - Ajouter le dépôt distant (GitHub)
1. Copiez l'URL HTTPS du dépôt GitHub.
2. Dans votre projet local, exécutez les commandes suivantes pour configurer la branche principale et ajouter le dépôt distant :
   ```bash
   git branch -M main
   git remote add origin <URL-HTTPS>
   git push origin main
   ```
3. Lorsqu'on vous le demande, fournissez votre nom d'utilisateur GitHub et le **token** comme mot de passe.

### 4 - Vérifier la synchronisation
Vérifiez sur GitHub que la branche principale (`main`) a bien été créée et que tous les fichiers locaux sont disponibles sur le dépôt distant.

#### [⬆️ Retour à la table des matières](#table-des-matieres)
<br/>

---

<a name="connexion-a-github-via-des-cles-ssh"></a>
## 5 - Connexion à GitHub via des clés SSH

### 1 - Créer des clés SSH sur le système local
Générez une paire de clés SSH sur votre système.
```bash
ssh-keygen
```
Copiez le contenu du fichier public :
```bash
cat ~/.ssh/id_rsa.pub
```

### 2 - Ajouter la clé SSH sur GitHub
1. Allez dans les paramètres de votre dépôt GitHub.
2. Dans **Settings**, sélectionnez **Deploy Keys**.
3. Cliquez sur **Add key**, collez la clé publique SSH, cochez **Allow write access**, puis cliquez sur **Add key**.

### 3 - Modifier la configuration du dépôt local
Remplacez l'URL HTTPS par l'URL SSH dans le fichier `.git/config` du dépôt local.
```bash
vi .git/config
```
Remplacez l'URL par celle en SSH : `git@github.com:<username>/<repository>.git`.

### 4 - Ajouter un nouveau fichier et pousser les changements
1. Créez un nouveau fichier dans votre dépôt local :
   ```bash
   touch bal.java
   git add bal.java
   git commit -m "Ajout du fichier bal.java"
   ```
2. Poussez les modifications sur GitHub via SSH :
   ```bash
   git push origin main
   ```

#### [⬆️ Retour à la table des matières](#table-des-matieres)
<br/>

---
<a name="conclusion"></a>
# 6 - Conclusion
---

Ce guide vous a montré les commandes essentielles du workflow Git pour créer un dépôt local, gérer les commits, et synchroniser avec un dépôt distant comme GitHub, en utilisant HTTPS ou SSH. Avec ces étapes, vous pouvez efficacement gérer vos projets et collaborer avec d'autres développeurs sur GitHub. Ce cours couvre les commandes de base du workflow Git et vous guide à travers les processus locaux et distants.

#### [⬆️ Retour à la table des matières](#table-des-matieres)
<br/>

---

