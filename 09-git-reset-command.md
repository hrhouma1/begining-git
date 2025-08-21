---
title: "Chapitre 9 - Commande Git Reset"
description: "Découvrez comment utiliser Git Reset pour réinitialiser l'état de votre dépôt à une version antérieure"
---



---
<a name="table-des-matieres"></a>

## Table des matières
---

1. [Introduction](#introduction)
2. [Partie 1 : Théorie](#partie-1-theorie)
   1. [Qu'est-ce que `git reset` ?](#qu-est-ce-que-git-reset)
   2. [Les trois modes principaux de Git Reset](#les-trois-modes-principaux-de-git-reset)
   3. [Quand utiliser `git reset` ?](#quand-utiliser-git-reset)
3. [Partie 2 : Pratique](#partie-2-pratique)
   1. [Étape 1 : Cloner le projet existant](#etape-1)
   2. [Étape 2 : Créer une nouvelle branche et faire plusieurs commits](#etape-2)
   3. [Étape 3 : Utiliser `git reset --soft`](#etape-3)
   4. [Étape 4 : Utiliser `git reset --mixed`](#etape-4)
   5. [Étape 5 : Utiliser `git reset --hard`](#etape-5)
   6. [Étape 6 : Pousser la branche vers GitHub](#etape-6)
4. [Résumé des commandes](#resume-commandes)
5. [Conclusion](#conclusion)

<br/>
---

<a name="introduction"></a>
## Introduction
---

La commande `git reset` est l'une des plus puissantes de Git, mais elle peut aussi être dangereuse si elle est mal utilisée. Ce guide va vous expliquer **qu'est-ce que git reset**, comment il fonctionne, et comment l'utiliser de manière sécurisée avec des exemples pratiques sur un projet que nous créerons ensemble.


[⬆️ retour à la table des matières](#table-des-matieres)
<br/>

---

<a name="partie-1-theorie"></a>
## 1 - Partie 1 : Théorie
---

<a name="qu-est-ce-que-git-reset"></a>
### **Qu'est-ce que `git reset` ?**

`git reset` est une commande Git qui permet de réinitialiser l'état de votre dépôt à une version antérieure. Il modifie la position du pointeur de la branche actuelle et, en fonction des options utilisées, peut également modifier les fichiers dans la zone de staging et/ou dans le répertoire de travail.

[⬆️ retour à la table des matières](#table-des-matieres)
<br/>

<a name="les-trois-modes-principaux-de-git-reset"></a>
### **Les trois modes principaux de Git Reset :**

1. **`--soft`** : Réinitialise uniquement le pointeur HEAD vers un commit antérieur, mais laisse les modifications dans la zone de staging. Cela signifie que vous pouvez créer un nouveau commit avec ces changements.

2. **`--mixed` (par défaut)** : Réinitialise le pointeur HEAD et enlève les fichiers de la zone de staging (les modifications restent dans votre répertoire de travail). Vous devrez utiliser `git add` si vous voulez les committer à nouveau.

3. **`--hard`** : Réinitialise le pointeur HEAD, la zone de staging et le répertoire de travail. Cela supprime toutes les modifications non committées, y compris les fichiers modifiés.

[⬆️ retour à la table des matières](#table-des-matieres)
<br/>

<a name="quand-utiliser-git-reset"></a>
### **Quand utiliser `git reset` ?**
- **Corriger un commit récent** : Si vous avez fait un commit par erreur ou vous souhaitez changer quelque chose dans le dernier commit.
- **Annuler des modifications locales** : Pour annuler des modifications dans la zone de staging ou dans le répertoire de travail.
- **Nettoyer l'historique** : Pour revenir à un état antérieur dans l'historique Git.

[⬆️ retour à la table des matières](#table-des-matieres)
<br/>

---

<a name="partie-2-pratique"></a>
## 2 - Partie 2 : Pratique
---

Nous allons maintenant voir comment utiliser `git reset` dans un cas concret en créant un projet exemple simple.

<a name="etape-1"></a>
### **Étape 1 : Créer un projet exemple pour git reset**

Nous allons créer un projet simple pour démontrer les différents modes de `git reset`.

```bash
mkdir projet-reset-demo
cd projet-reset-demo
git init
```

**Structure du projet :**
```
projet-reset-demo/
├── README.md
├── script.py
├── config.json
└── utils.js
```

Créons nos fichiers de base :

```bash
# Créer les fichiers du projet
echo "# Projet de démonstration Git Reset" > README.md
echo "print('Script principal')" > script.py
echo '{"version": "1.0.0"}' > config.json
echo "// Fonctions utilitaires" > utils.js

# Premier commit initial
git add .
git commit -m "Structure initiale du projet"
```

---   

[⬆️ retour à la table des matières](#table-des-matieres)
<br/>

<a name="etape-2"></a>
### **Étape 2 : Créer une nouvelle branche et faire plusieurs commits**

1. **Créer une nouvelle branche pour expérimenter avec `git reset`** :
   ```bash
   git checkout -b experiment-reset
   ```

2. **Effectuer plusieurs petites modifications et commits** :

   - **Premier commit** : Améliorons le fichier `script.py` :
   
     ```bash
     echo "
def saluer(nom):
    print(f'Bonjour {nom}!')

saluer('Monde')" >> script.py
     ```

     Ensuite, ajoutez le fichier à la zone de staging et créez un commit :
     ```bash
     git add script.py
     git commit -m "Ajout fonction saluer dans script.py"
     ```

   - **Deuxième commit** : Mettons à jour la configuration :
   
     ```bash
     echo '{
  "version": "1.1.0",
  "auteur": "Mon Nom",
  "debug": true
}' > config.json
     ```

     Ensuite, créez un commit :
     ```bash
     git add config.json
     git commit -m "Mise à jour configuration version 1.1.0"
     ```

   - **Troisième commit** : Améliorons les utilitaires JavaScript :
   
     ```bash
     echo "
function calculer(a, b) {
    return a + b;
}

console.log('Utilitaires chargés');" >> utils.js
     ```

     Puis, créez un commit :
     ```bash
     git add utils.js
     git commit -m "Ajout fonction calcul dans utils.js"
     ```

3. **Vérifier l'historique des commits** :
   
   Exécutez cette commande pour voir les trois commits que vous avez créés :
   ```bash
   git log --oneline
   ```

   Vous devriez voir quelque chose comme ça :
   ```
   a7f3e2b Ajout fonction calcul dans utils.js
   9c1d8a4 Mise à jour configuration version 1.1.0  
   5e9b6f1 Ajout fonction saluer dans script.py
   2x4y8z3 Structure initiale du projet
   ```

**Représentation ASCII de l'historique actuel :**
```
experiment-reset
    ↓
    * a7f3e2b (HEAD) Ajout fonction calcul dans utils.js
    ↓
    * 9c1d8a4 Mise à jour configuration version 1.1.0
    ↓
    * 5e9b6f1 Ajout fonction saluer dans script.py
    ↓
    * 2x4y8z3 (main) Structure initiale du projet
```

---
[⬆️ retour à la table des matières](#table-des-matieres)
<br/>


<a name="etape-3-utiliser-git-reset-soft"></a>
### **Étape 3 : Utiliser `git reset --soft`**

#### **Cas d’utilisation :**

Supposons que vous réalisiez que vous ne voulez pas avoir trois commits séparés pour ces petites modifications. Vous voulez les combiner en un seul commit, mais vous ne voulez pas perdre vos changements actuels.

1. **Réinitialiser à un commit précédent avec `--soft`** :

   Vous pouvez utiliser `git reset --soft` pour revenir en arrière dans l'historique tout en gardant vos changements dans la zone de staging. Ici, nous allons revenir avant le **premier commit**.

   Exécutez la commande suivante pour revenir à l’état avant le premier commit (en utilisant l'ID du commit approprié) :
   ```bash
   git reset --soft HEAD~3
   ```

2. **Vérifier l’état** :

   Après avoir exécuté `git reset --soft`, tapez `git status` pour voir que tous vos fichiers sont toujours dans la zone de staging, prêts à être committés.

3. **Créer un nouveau commit** :

   Maintenant, vous pouvez créer un nouveau commit unique qui combine les trois précédents :
   ```bash
   git commit -m "Regroupement des changements pour app.js, index.php, et connection.php"
   ```

---

[⬆️ retour à la table des matières](#table-des-matieres)
<br/>

<a name="etape-4-utiliser-git-reset-mixed"></a>
### **Étape 4 : Utiliser `git reset --mixed`**

#### **Cas d’utilisation :**

Supposons que vous avez ajouté des fichiers à la zone de staging par erreur et que vous voulez les retirer, mais vous ne voulez pas perdre les modifications que vous avez faites. `git reset --mixed` est utile pour cela.

1. **Ajouter `script.py` à la zone de staging** :
   ```bash
   git add script.py
   ```

2. **Réinitialiser la zone de staging avec `--mixed`** :
   ```bash
   git reset --mixed
   ```

3. **Vérifier l'état** :

   Tapez `git status` pour voir que `script.py` n'est plus dans la zone de staging, mais les modifications locales sont toujours présentes dans le répertoire de travail.

---

[⬆️ retour à la table des matières](#table-des-matieres)
<br/>

<a name="etape-5-utiliser-git-reset-hard"></a>
### **Étape 5 : Utiliser `git reset --hard`**

#### **Cas d’utilisation :**

Supposons que vous ayez modifié plusieurs fichiers et que vous réalisez que vous voulez tout annuler. `git reset --hard` efface toutes les modifications locales dans la zone de staging et dans votre répertoire de travail.

**Attention : Cette commande est dangereuse, car elle supprime toutes les modifications non committées.**

1. **Faire une modification dans `config.json`** :
   
   Modifiez `config.json` pour y ajouter une nouvelle propriété :
   ```bash
   echo '{
  "version": "1.2.0",
  "auteur": "Mon Nom",
  "debug": true,
  "test": "MODIFICATION NON DÉSIRÉE"
}' > config.json
   ```

2. **Vérifier l'état** :
   ```bash
   git status
   ```

   Vous verrez que `config.json` est marqué comme modifié.

3. **Réinitialiser avec `--hard`** :

   Exécutez la commande suivante pour annuler la modification et revenir à l'état du dernier commit :
   ```bash
   git reset --hard
   ```

4. **Vérifier que la modification est annulée** :

   Ouvrez `config.json` et constatez que la propriété "test" ajoutée a été supprimée. Vous pouvez également exécuter `git status` pour vérifier que tout est revenu à l'état initial.

---

[⬆️ retour à la table des matières](#table-des-matieres)
<br/>

<a name="etape-6"></a>
### **Étape 6 : Finaliser et intégrer (Optionnel)**

Après avoir expérimenté avec les différentes options de `git reset`, vous pouvez finaliser votre travail :

1. **Basculer vers la branche principale** :
   ```bash
   git checkout main
   ```

2. **Fusionner vos expérimentations** (si vous le souhaitez) :
   ```bash
   git merge experiment-reset
   ```

3. **Nettoyer en supprimant la branche d'expérimentation** :
   ```bash
   git branch -d experiment-reset
   ```

**Représentation ASCII finale :**
```
main
  ↓
  * a7f3e2b Ajout fonction calcul dans utils.js
  ↓
  * 9c1d8a4 Mise à jour configuration version 1.1.0
  ↓  
  * 5e9b6f1 Ajout fonction saluer dans script.py
  ↓
  * 2x4y8z3 Structure initiale du projet
```

*Optionnel : Si vous travaillez avec un dépôt distant :*
```bash
git push origin main
```


[⬆️ retour à la table des matières](#table-des-matieres)
<br/>

---


<a name="resume-commandes"></a>
## 3 - Résumé des commandes
---

### **Commandes complètes du tutoriel**

1. **Créer le projet exemple** :
   ```bash
   mkdir projet-reset-demo && cd projet-reset-demo
   git init
   echo "# Projet de démonstration Git Reset" > README.md
   echo "print('Script principal')" > script.py
   echo '{"version": "1.0.0"}' > config.json
   echo "// Fonctions utilitaires" > utils.js
   git add . && git commit -m "Structure initiale du projet"
   ```

2. **Créer une branche d'expérimentation** :
   ```bash
   git checkout -b experiment-reset
   ```

3. **Effectuer plusieurs commits** :
   ```bash
   # Commit 1
   echo "def saluer(nom): print(f'Bonjour {nom}!')" >> script.py
   git add script.py && git commit -m "Ajout fonction saluer dans script.py"
   
   # Commit 2
   echo '{"version": "1.1.0", "auteur": "Mon Nom", "debug": true}' > config.json
   git add config.json && git commit -m "Mise à jour configuration version 1.1.0"
   
   # Commit 3
   echo "function calculer(a, b) { return a + b; }" >> utils.js
   git add utils.js && git commit -m "Ajout fonction calcul dans utils.js"
   ```

### **Commandes essentielles Git Reset**

4. **Utiliser `git reset --soft`** (garde les fichiers dans staging) :
   ```bash
   git reset --soft HEAD~3
   ```

5. **Utiliser `git reset --mixed`** (par défaut, retire de staging) :
   ```bash
   git reset --mixed
   ```

6. **Utiliser `git reset --hard`** (ATTENTION: supprime tout) :
   ```bash
   git reset --hard
   ```

### **Commandes de visualisation**
```bash
git log --oneline          # Voir l'historique
git status                 # Vérifier l'état
git log --oneline --graph  # Historique graphique
```

#### [⬆️ retour à la table des matières](#table-des-matieres)
<br/>

---

<a name="conclusion"></a>
### 4 - **Conclusion**

**Félicitations !** Ce guide vous a montré comment utiliser les trois modes principaux de **Git Reset** avec un projet exemple complet et autonome.

**Ce que vous avez appris :**
- Créer un projet Git avec plusieurs commits
- Comprendre les différences entre `--soft`, `--mixed`, et `--hard`
- Visualiser l'impact de chaque mode sur l'historique Git
- Utiliser Git Reset de manière sécurisée

**Récapitulatif des modes :**
- **--soft** : Réinitialise HEAD, garde staging + working directory
- **--mixed** : Réinitialise HEAD + staging, garde working directory  
- **--hard** : Réinitialise tout (ATTENTION: destructif!)

**Bonnes pratiques :**
- Toujours vérifier `git status` avant et après un reset
- Utiliser `--soft` pour regrouper des commits
- Utiliser `--mixed` pour retirer des fichiers du staging
- Utiliser `--hard` avec prudence (modifications perdues!)

Vous pouvez désormais utiliser Git Reset efficacement tout en évitant les pièges courants !

#### [⬆️ retour à la table des matières](#table-des-matieres)
<br/>

---

