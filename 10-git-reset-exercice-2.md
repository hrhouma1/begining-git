---
title: "Chapitre 10 - Application de la commande Git Reset"
description: "Découvrez comment utiliser Git Reset pour réinitialiser l'état de votre dépôt à une version antérieure"
---

---
<a name="table-des-matieres"></a>

## Table des matières
---

### **Objectif :**
Ce guide vous apprendra à utiliser les trois modes principaux de la commande `git reset` : **--soft**, **--mixed**, et **--hard**. Chaque mode a un effet différent sur les commits, la zone de staging, et votre répertoire de travail. Vous apprendrez à réinitialiser l'état de votre dépôt Git et à comprendre les impacts de chaque mode sur vos fichiers.

---

## **Partie 1 : Théorie**

### **Les trois modes principaux de Git Reset :**

1. **`git reset --soft`** :
   - **Effet** : Réinitialise le pointeur **HEAD** (le commit auquel vous êtes actuellement), mais garde les modifications dans la zone de staging.
   - **Quand l’utiliser ?** : Utilisez ce mode si vous voulez refaire un commit sans perdre vos changements, par exemple pour corriger un message de commit ou regrouper plusieurs commits en un seul.
  
2. **`git reset --mixed` (par défaut)** :
   - **Effet** : Réinitialise **HEAD** et enlève les fichiers de la zone de staging, mais conserve les modifications dans votre répertoire de travail.
   - **Quand l’utiliser ?** : Utilisez ce mode si vous avez ajouté des fichiers à la zone de staging par erreur et souhaitez les retirer sans perdre les modifications dans votre répertoire de travail.

3. **`git reset --hard`** :
   - **Effet** : Réinitialise **HEAD**, la zone de staging, et le répertoire de travail. Cela supprime toutes les modifications non committées.
   - **Quand l’utiliser ?** : Utilisez ce mode avec prudence, car il supprime toutes les modifications locales non enregistrées. C'est utile si vous voulez annuler complètement des modifications et revenir à un état antérieur.

---

## **Partie 2 : Pratique**

### **Étape 1 : Créer un projet exemple pour tester Git Reset**

Nous allons créer un projet simple pour démontrer les trois modes de `git reset`.

```bash
mkdir projet-reset-exercice
cd projet-reset-exercice
git init
```

**Structure du projet :**
```
projet-reset-exercice/
├── README.md
├── main.py
├── style.css
└── app.js
```

Créons nos fichiers de base :

```bash
# Créer les fichiers du projet
echo "# Exercice Git Reset" > README.md
echo "print('Application principale')" > main.py
echo "body { font-family: Arial; }" > style.css
echo "console.log('App démarrée');" > app.js

# Premier commit initial
git add .
git commit -m "Initialisation du projet exercice"
```

---

### **Étape 2 : Créer une nouvelle branche pour tester `git reset`**

Nous allons créer une nouvelle branche pour faire des modifications et tester les différents modes de `git reset`.

1. Créez une nouvelle branche :
   
   ```bash
   git checkout -b reset-test-branch
   ```

---

### **Étape 3 : Faire plusieurs modifications et commits**

Nous allons maintenant faire des petites modifications dans les fichiers, créer plusieurs commits, et ensuite utiliser `git reset` pour voir comment cela fonctionne.

#### **1. Modifier et committer `main.py` :**

1. Ajoutons une fonction dans le fichier `main.py` :
   
   ```bash
   echo "
def test_soft_reset():
    print('Test de git reset --soft')

test_soft_reset()" >> main.py
   ```

2. Ensuite, ajoutez le fichier à la zone de staging et créez un commit :
   
   ```bash
   git add main.py
   git commit -m "Ajout fonction test_soft_reset dans main.py"
   ```

#### **2. Modifier et committer `style.css` :**

1. Ajoutons des styles pour tester git reset --mixed :
   
   ```bash
   echo "
/* Styles pour test git reset --mixed */
.container {
    max-width: 800px;
    margin: 0 auto;
}" >> style.css
   ```

2. Ensuite, ajoutez le fichier à la zone de staging et créez un commit :
   
   ```bash
   git add style.css
   git commit -m "Ajout styles container pour test git reset --mixed"
   ```

#### **3. Modifier et committer `app.js` :**

1. Ajoutons une fonction JavaScript pour tester git reset --hard :
   
   ```bash
   echo "
// Test pour git reset --hard
function testHardReset() {
    console.log('Test de git reset --hard');
}

testHardReset();" >> app.js
   ```

2. Ensuite, ajoutez le fichier à la zone de staging et créez un commit :
   
   ```bash
   git add app.js
   git commit -m "Ajout fonction testHardReset dans app.js"
   ```

#### **4. Vérifier l'historique des commits :**

Tapez la commande suivante pour voir l’historique des trois commits que nous venons de créer :

```bash
git log --oneline
```

Vous devriez voir un résultat comme celui-ci :

```
c4f8e2a Ajout fonction testHardReset dans app.js
b7d3f1e Ajout styles container pour test git reset --mixed
9a5e6c2 Ajout fonction test_soft_reset dans main.py
1x2y3z4 Initialisation du projet exercice
```

**Représentation ASCII de l'historique :**
```
reset-test-branch
    ↓
    * c4f8e2a (HEAD) Ajout fonction testHardReset dans app.js
    ↓
    * b7d3f1e Ajout styles container pour test git reset --mixed  
    ↓
    * 9a5e6c2 Ajout fonction test_soft_reset dans main.py
    ↓
    * 1x2y3z4 (main) Initialisation du projet exercice
```

---

### **Étape 4 : Utiliser `git reset --soft`**

**But :** Nous allons utiliser `git reset --soft` pour revenir avant le premier commit, tout en gardant les fichiers dans la zone de staging. Cela nous permettra de modifier le commit ou de regrouper plusieurs commits en un seul.

1. **Réinitialiser avec `--soft`** :

   Exécutez la commande suivante pour réinitialiser à l'état avant le premier commit :

   ```bash
   git reset --soft HEAD~3
   ```

2. **Vérifier l'état :**

   Tapez `git status`. Vous verrez que tous vos fichiers modifiés sont toujours dans la zone de staging, prêts à être committés de nouveau. Vous pouvez maintenant les committer à nouveau ou les regrouper en un seul commit.

3. **Créer un nouveau commit** :

   Créez un nouveau commit unique qui combine les trois précédents :

   ```bash
   git commit -m "Regroupement des modifications pour app.js, index.php, et connection.php"
   ```

---

### **Étape 5 : Utiliser `git reset --mixed`**

**But :** `git reset --mixed` enlève les fichiers de la zone de staging, mais garde les modifications dans le répertoire de travail.

1. **Ajoutez `main.py` à la zone de staging** :

   ```bash
   git add main.py
   ```

2. **Réinitialiser avec `--mixed`** :

   Exécutez la commande suivante pour retirer le fichier `main.py` de la zone de staging tout en gardant les modifications locales :

   ```bash
   git reset --mixed
   ```

3. **Vérifier l'état :**

   Tapez `git status`. Vous verrez que `main.py` est retiré de la zone de staging, mais les modifications sont toujours présentes dans le répertoire de travail. Si vous regardez dans l'éditeur, vous verrez toujours la fonction ajoutée dans `main.py`.

---

### **Étape 6 : Utiliser `git reset --hard`**

**But :** `git reset --hard` réinitialise tout : HEAD, la zone de staging, et le répertoire de travail. Cela supprime toutes les modifications non committées. **Attention, cette commande est irréversible !**

1. **Modifiez `style.css` :**

   Ajoutons une modification non désirée dans le fichier `style.css` :
   
   ```bash
   echo "
/* MODIFICATION NON DESIRÉE */
.unwanted {
    color: red !important;
}" >> style.css
   ```

2. **Réinitialiser avec `--hard`** :

   Exécutez la commande suivante pour annuler toutes les modifications locales, y compris celle que vous venez de faire dans `style.css` :

   ```bash
   git reset --hard
   ```

3. **Vérifier l'état :**

   Ouvrez à nouveau `style.css` dans votre éditeur. Vous verrez que les lignes ajoutées ont été supprimées et que toutes les modifications locales ont été annulées.

---

### **Étape 7 : Finaliser l'exercice**

Après avoir expérimenté avec tous les modes de `git reset`, finalisez votre travail :

1. **Basculer vers la branche principale** :
   ```bash
   git checkout main
   ```

2. **Fusionner vos expérimentations** (optionnel) :
   ```bash
   git merge reset-test-branch
   ```

3. **Nettoyer en supprimant la branche de test** :
   ```bash
   git branch -d reset-test-branch
   ```

*Optionnel : Si vous travaillez avec un dépôt distant :*
```bash
git push origin main
```

---

## **Résumé des commandes**

### **Commandes complètes de l'exercice**

1. **Créer le projet exercice** :
   ```bash
   mkdir projet-reset-exercice && cd projet-reset-exercice
   git init
   echo "# Exercice Git Reset" > README.md
   echo "print('Application principale')" > main.py
   echo "body { font-family: Arial; }" > style.css
   echo "console.log('App démarrée');" > app.js
   git add . && git commit -m "Initialisation du projet exercice"
   ```

2. **Créer une branche de test** :
   ```bash
   git checkout -b reset-test-branch
   ```

3. **Effectuer plusieurs commits** :
   ```bash
   # Commit 1
   echo "def test_soft_reset(): print('Test de git reset --soft')" >> main.py
   git add main.py && git commit -m "Ajout fonction test_soft_reset dans main.py"
   
   # Commit 2
   echo ".container { max-width: 800px; margin: 0 auto; }" >> style.css
   git add style.css && git commit -m "Ajout styles container pour test git reset --mixed"
   
   # Commit 3
   echo "function testHardReset() { console.log('Test de git reset --hard'); }" >> app.js
   git add app.js && git commit -m "Ajout fonction testHardReset dans app.js"
   ```

### **Commandes des différents modes Git Reset**

4. **Utiliser `git reset --soft`** (garde fichiers en staging) :
   ```bash
   git reset --soft HEAD~3
   ```

5. **Utiliser `git reset --mixed`** (retire de staging) :
   ```bash
   git reset --mixed
   ```

6. **Utiliser `git reset --hard`** (ATTENTION: supprime tout) :
   ```bash
   git reset --hard
   ```

### **Commandes de finalisation**
```bash
git checkout main             # Retour branche principale
git merge reset-test-branch   # Fusionner si désiré
git branch -d reset-test-branch  # Supprimer branche test
```

---

## **Conclusion**

**Félicitations !** Vous venez de terminer l'exercice pratique sur **Git Reset** avec un projet exemple complet et autonome.

**Ce que vous avez maîtrisé :**
- Création d'un projet Git multi-fichiers
- Application pratique des trois modes de reset
- Compréhension des effets sur HEAD, staging et working directory
- Visualisation des changements dans l'historique Git

**Récapitulatif pratique :**
- **git reset --soft** : Idéal pour regrouper des commits
- **git reset --mixed** : Parfait pour retirer des fichiers du staging  
- **git reset --hard** : Puissant mais destructeur, à utiliser avec précaution

**Points clés à retenir :**
- Toujours vérifier `git status` après un reset
- `--hard` est irréversible (modifications perdues définitivement)
- Les diagrammes ASCII vous aident à visualiser l'état du dépôt
- Tester sur des branches évite les problèmes sur main

Vous êtes maintenant capable d'utiliser Git Reset efficacement dans vos projets !

---

