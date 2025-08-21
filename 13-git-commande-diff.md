---
title: "Chapitre 13 - Commande Git Diff"
description: "Découvrez comment utiliser la commande Git Diff pour visualiser les différences entre deux états d'un fichier dans un projet Git"
---

---
<a name="table-des-matieres"></a>

## Table des matières
---

### **Objectif :**
Ce guide vous apprendra à utiliser la commande `git diff`, qui permet de visualiser les différences entre deux états d'un fichier dans un projet Git. Vous verrez comment cette commande est utile pour comparer les modifications que vous avez apportées avant de les committer.

---

## **Partie 1 : Théorie**

### **Qu'est-ce que `git diff` ?**

- **`git diff`** est une commande Git qui permet de comparer les différences entre différentes versions d'un fichier. Cela peut inclure des modifications locales qui ne sont pas encore committées, des différences entre branches, ou des différences entre des commits dans l'historique.

### **Quand utiliser `git diff` ?**

- **Avant un commit** : Pour vérifier les changements que vous avez effectués dans vos fichiers avant de les ajouter à la zone de staging ou de les committer.
- **Entre branches** : Pour voir les différences entre deux branches avant de les fusionner.
- **Entre commits** : Pour comparer deux commits spécifiques dans l'historique Git.

### **Types de différences que vous pouvez comparer avec `git diff` :**
1. **Différences dans le répertoire de travail** : Comparez les fichiers modifiés mais non ajoutés à la zone de staging.
2. **Différences entre la zone de staging et le répertoire de travail** : Comparez les fichiers dans la zone de staging avec ceux du répertoire de travail.
3. **Différences entre commits** : Comparez des versions différentes de fichiers à travers les commits.

---

## **Partie 2 : Pratique**

Nous allons maintenant voir comment utiliser la commande `git diff` avec un projet exemple que nous créerons ensemble.

### **Étape 1 : Créer un projet exemple pour git diff**

Créons un projet simple pour démontrer les différentes utilisations de `git diff`.

```bash
mkdir projet-diff-demo
cd projet-diff-demo
git init
```

**Structure du projet :**
```
projet-diff-demo/
├── README.md
├── calculator.py
├── styles.css
└── app.js
```

Créons nos fichiers de base :

```bash
# Créer les fichiers du projet
echo "# Projet de démonstration Git Diff" > README.md
echo "def add(a, b):
    return a + b

def subtract(a, b):
    return a - b" > calculator.py

echo "body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 20px;
}" > styles.css

echo "// Application principale
console.log('App démarrée');

function init() {
    console.log('Initialisation terminée');
}" > app.js

# Premier commit initial
git add .
git commit -m "Version initiale du projet"
```

---

### **Étape 2 : Créer une nouvelle branche pour tester `git diff`**

Nous allons créer une nouvelle branche pour faire des modifications et tester la commande `git diff`.

1. Créez une nouvelle branche appelée `diff-test-branch` :

   ```bash
   git checkout -b diff-test-branch
   ```

---

### **Étape 3 : Faire des modifications dans plusieurs fichiers**

Nous allons faire quelques modifications dans différents fichiers afin de voir comment `git diff` nous permet de visualiser ces différences.

#### **1. Modifier `calculator.py` :**

1. Ajoutons une nouvelle fonction dans `calculator.py` :

   ```bash
   echo "
def multiply(a, b):
    \"\"\"Multiplie deux nombres\"\"\"
    return a * b

def divide(a, b):
    \"\"\"Divise deux nombres\"\"\" 
    if b != 0:
        return a / b
    return 'Erreur: division par zéro'" >> calculator.py
   ```

2. **Ne commitez pas encore !** Nous allons d'abord utiliser `git diff` pour voir les changements.

#### **2. Modifier `styles.css` :**

1. Ajoutons des styles pour un thème sombre :

   ```bash
   echo "
/* Thème sombre */
.dark-theme {
    background-color: #1a1a1a;
    color: #ffffff;
}

.button {
    padding: 10px 15px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}" >> styles.css
   ```

2. **Ne commitez pas encore !** Nous utiliserons `git diff` pour analyser les modifications.

---

### **Étape 4 : Utiliser `git diff` pour comparer les modifications non ajoutées à la zone de staging**

Nous allons maintenant utiliser `git diff` pour voir les différences dans les fichiers modifiés mais non ajoutés à la zone de staging.

1. Tapez la commande suivante pour voir les différences dans **tous les fichiers modifiés** dans le répertoire de travail :

   ```bash
   git diff
   ```

   Vous verrez les changements que vous avez faits dans `calculator.py` et `styles.css`. Chaque ligne ajoutée sera affichée en **vert** avec un signe `+`.

**Exemple de sortie attendue :**
```diff
diff --git a/calculator.py b/calculator.py
index 1a2b3c4..5d6e7f8 100644
--- a/calculator.py
+++ b/calculator.py
@@ -4,0 +5,9 @@ def subtract(a, b):
+
+def multiply(a, b):
+    """Multiplie deux nombres"""
+    return a * b
+
+def divide(a, b):
+    """Divise deux nombres"""
+    if b != 0:
+        return a / b
+    return 'Erreur: division par zéro'
```

---

### **Étape 5 : Ajouter les modifications à la zone de staging**

Maintenant, nous allons ajouter les fichiers à la zone de staging pour voir comment `git diff` compare les fichiers dans la zone de staging avec ceux du répertoire de travail.

1. Ajoutez **uniquement `calculator.py`** à la zone de staging :

   ```bash
   git add calculator.py
   ```

---

### **Étape 6 : Utiliser `git diff` pour comparer la zone de staging et le répertoire de travail**

Après avoir ajouté `app.js` à la zone de staging, nous allons comparer la différence entre la version dans la zone de staging et la version non ajoutée dans le répertoire de travail.

1. Tapez la commande suivante pour comparer les fichiers dans la zone de staging et les fichiers modifiés dans le répertoire de travail :

   ```bash
   git diff
   ```

   Comme `calculator.py` est déjà dans la zone de staging, **aucune différence** ne sera affichée pour ce fichier. Cependant, les modifications dans `styles.css` (qui ne sont pas encore ajoutées à la zone de staging) seront affichées.

2. Pour comparer uniquement ce qui est dans la zone de staging, utilisez :

   ```bash
   git diff --staged
   ```

   Cette commande vous montrera uniquement les différences des fichiers déjà ajoutés à la zone de staging, comme `calculator.py`.

**Visualisation des zones :**
```
Working Directory    Staging Area       Repository
-----------------   --------------     ------------
calculator.py  ←→   calculator.py  →   (dernier commit)
styles.css [M]      (vide)             (dernier commit)

[M] = Modified (modifié mais pas en staging)
```

---

### **Étape 7 : Utiliser `git diff` entre deux commits**

Nous allons maintenant committer nos modifications, puis comparer les différences entre deux commits spécifiques.

1. **Commettre les modifications** :

   - Ajoutez `styles.css` à la zone de staging :
   
     ```bash
     git add styles.css
     ```

   - Créez un commit avec les modifications dans `calculator.py` et `styles.css` :
   
     ```bash
     git commit -m "Ajout fonctions multiply/divide et styles thème sombre"
     ```

2. **Vérifier l'historique des commits** :

   Tapez la commande suivante pour voir les ID des commits récents :

   ```bash
   git log --oneline
   ```

   Vous verrez les ID des commits récents :
   ```
   f8a2e5c Ajout fonctions multiply/divide et styles thème sombre
   3b1d9a7 Version initiale du projet
   ```

**Représentation ASCII de l'historique :**
```
diff-test-branch
    ↓
    * f8a2e5c (HEAD) Ajout fonctions multiply/divide et styles thème sombre
    ↓
    * 3b1d9a7 Version initiale du projet
```

3. **Comparer deux commits spécifiques** :

   Pour voir toutes les différences entre le commit initial et le commit actuel :

   ```bash
   git diff 3b1d9a7 f8a2e5c
   ```

4. **Comparer avec HEAD (commit actuel)** :
   
   Pour comparer le commit précédent avec HEAD :
   ```bash
   git diff HEAD~1 HEAD
   ```

5. **Comparer un fichier spécifique entre commits** :
   
   Pour voir seulement les changements dans `calculator.py` :
   ```bash
   git diff 3b1d9a7 f8a2e5c calculator.py
   ```

---

### **Étape 8 : Finaliser l'exercice (Optionnel)**

Après avoir expérimenté avec `git diff`, vous pouvez finaliser votre projet :

1. **Basculer vers main et fusionner** :
   ```bash
   git checkout main
   git merge diff-test-branch
   ```

2. **Nettoyer la branche de test** :
   ```bash
   git branch -d diff-test-branch
   ```

**Représentation ASCII finale :**
```
main (après fusion)
    ↓
    * f8a2e5c Ajout fonctions multiply/divide et styles thème sombre
    ↓
    * 3b1d9a7 Version initiale du projet
```

*Optionnel : Si vous travaillez avec un dépôt distant :*
```bash
git push origin main
```

---

## **Résumé des commandes**

### **Commandes complètes du tutoriel**

1. **Créer le projet exemple** :
   ```bash
   mkdir projet-diff-demo && cd projet-diff-demo
   git init
   echo "# Projet de démonstration Git Diff" > README.md
   echo "def add(a, b): return a + b" > calculator.py
   echo "body { font-family: Arial; margin: 0; }" > styles.css
   echo "console.log('App démarrée');" > app.js
   git add . && git commit -m "Version initiale du projet"
   ```

2. **Créer une branche de test** :
   ```bash
   git checkout -b diff-test-branch
   ```

3. **Effectuer des modifications** :
   ```bash
   # Ajouter fonctions dans calculator.py
   echo "def multiply(a, b): return a * b" >> calculator.py
   echo "def divide(a, b): return a/b if b!=0 else 'Erreur'" >> calculator.py
   
   # Ajouter styles dans styles.css  
   echo ".dark-theme { background: #1a1a1a; color: #fff; }" >> styles.css
   echo ".button { padding: 10px; border: none; }" >> styles.css
   ```

### **Commandes essentielles Git Diff**

4. **Voir différences dans working directory** :
   ```bash
   git diff                    # Toutes les modifications non stagées
   git diff calculator.py      # Modifications d'un fichier spécifique
   ```

5. **Gérer la zone de staging** :
   ```bash
   git add calculator.py       # Ajouter à staging
   git diff                    # Voir ce qui reste non stagé
   git diff --staged           # Voir ce qui est en staging
   ```

6. **Comparer les commits** :
   ```bash
   git commit -m "Nouvelles fonctionnalités"
   git log --oneline           # Voir les IDs des commits
   git diff HEAD~1 HEAD        # Comparer avec commit précédent
   git diff <commit1> <commit2> # Comparer deux commits spécifiques
   git diff HEAD~1 HEAD calculator.py  # Diff d'un fichier entre commits
   ```

### **Options utiles de git diff**
```bash
git diff --color-words       # Diff mot par mot (plus lisible)
git diff --stat             # Statistiques des changements
git diff --name-only        # Seulement les noms des fichiers modifiés
git diff --cached           # Alias pour --staged
```

---

## **Conclusion**

**Félicitations !** Vous avez maîtrisé la commande **Git Diff** avec un projet exemple complet et autonome.

**Ce que vous avez appris :**
- Créer un projet Git avec plusieurs types de fichiers
- Utiliser `git diff` dans différents contextes (working, staging, commits)
- Interpréter la sortie colorée de git diff
- Comparer des fichiers spécifiques entre différentes versions
- Naviguer efficacement entre les zones Git (working, staging, repository)

**Types de comparaisons Git Diff :**
- **`git diff`** : Working directory ↔ Staging area
- **`git diff --staged`** : Staging area ↔ Repository (dernier commit)
- **`git diff HEAD~1 HEAD`** : Entre commits (précédent ↔ actuel)
- **`git diff commit1 commit2`** : Entre deux commits spécifiques

**Cas d'usage pratiques :**
- **Avant commit** : Vérifier ce qu'on va committer
- **Code review** : Examiner les changements d'un collègue
- **Debug** : Comprendre quand un bug a été introduit
- **Revue de code** : Analyser l'évolution d'un fichier dans le temps

**Lecture de la sortie git diff :**
```diff
--- a/fichier.py    (version ancienne)
+++ b/fichier.py    (version nouvelle)
@@ -4,6 +4,8 @@    (ligne 4, 6 lignes → ligne 4, 8 lignes)
 def fonction():
     print("code existant")
-    ancienne_ligne       (supprimée, rouge)
+    nouvelle_ligne       (ajoutée, vert)  
+    autre_ajout         (ajoutée, vert)
```

**Options avancées utiles :**
- `--color-words` : Diff au niveau des mots (plus précis)
- `--stat` : Résumé statistique des changements
- `--name-only` : Liste seulement les fichiers modifiés
- `--ignore-whitespace` : Ignore les espaces/tabulations

Vous pouvez désormais utiliser Git Diff pour analyser précisément l'évolution de votre code !

---
