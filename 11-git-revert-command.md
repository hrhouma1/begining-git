
<a name="table-des-matieres"></a>

## Table des matières


### **Objectif :**
Ce guide vous apprendra à utiliser la commande `git revert`, qui permet d'annuler un ou plusieurs commits sans supprimer l'historique des modifications. Contrairement à `git reset`, `git revert` conserve les commits et ajoute un nouveau commit qui "inverse" les changements d'un commit spécifique.



## **Partie 1 : Théorie**

### **Qu'est-ce que `git revert` ?**

- **`git revert`** est une commande qui permet de revenir sur les changements apportés par un commit spécifique, en créant un nouveau commit "inverse". Contrairement à `git reset`, qui modifie l'historique en supprimant les commits, `git revert` conserve tous les commits précédents tout en annulant leurs effets. Cela rend la commande plus sûre à utiliser, surtout dans les environnements collaboratifs.

### **Quand utiliser `git revert` ?**
- **Corriger un commit erroné** : Si un commit a introduit un bug ou une erreur, vous pouvez utiliser `git revert` pour revenir en arrière tout en conservant l'historique des modifications.
- **Travail en équipe** : Lorsque vous collaborez avec d'autres développeurs, `git revert` est plus sûr que `git reset`, car il ne modifie pas l'historique partagé.
  


## **Partie 2 : Pratique**

Nous allons maintenant mettre en pratique l'utilisation de `git revert` dans un projet Git.

### **Étape 1 : Créer un projet exemple pour git revert**

Nous allons créer un projet simple pour démontrer le fonctionnement de `git revert`.

```bash
mkdir projet-revert-demo
cd projet-revert-demo
git init
```

**Structure du projet :**
```
projet-revert-demo/
├── README.md
├── calculator.py
├── styles.css
└── utils.js
```

Créons nos fichiers de base :

```bash
# Créer les fichiers du projet
echo "# Projet démonstration Git Revert" > README.md
echo "def add(a, b): return a + b" > calculator.py
echo "body { margin: 0; padding: 20px; }" > styles.css
echo "console.log('Utilitaires chargés');" > utils.js

# Premier commit initial
git add .
git commit -m "Initialisation du projet revert demo"
```



### **Étape 2 : Créer une nouvelle branche pour tester `git revert`**

Nous allons créer une nouvelle branche pour faire des modifications et tester la commande `git revert`.

1. Créez une nouvelle branche appelée `revert-test-branch` :

   ```bash
   git checkout -b revert-test-branch
   ```



### **Étape 3 : Faire plusieurs modifications et commits**

Nous allons maintenant faire des modifications dans le projet et créer plusieurs commits pour ensuite annuler l'un d'entre eux avec `git revert`.

#### **1. Modifier et committer `calculator.py` :**

1. Ajoutons une fonction multiplication dans `calculator.py` :

   ```bash
   echo "
def multiply(a, b):
    return a * b

print('Calculatrice mise à jour')" >> calculator.py
   ```

2. Ajoutez le fichier à la zone de staging et créez un commit :

   ```bash
   git add calculator.py
   git commit -m "Ajout fonction multiply dans calculator.py"
   ```

#### **2. Modifier et committer `styles.css` :**

1. Ajoutons des styles pour les boutons :

   ```bash
   echo "
/* Styles pour boutons */
.button {
    padding: 10px 20px;
    background: #007bff;
    color: white;
    border: none;
}" >> styles.css
   ```

2. Ajoutez le fichier à la zone de staging et créez un commit :

   ```bash
   git add styles.css
   git commit -m "Ajout styles boutons dans styles.css"
   ```

#### **3. Modifier et committer `utils.js` :**

1. Ajoutons des fonctions utilitaires JavaScript :

   ```bash
   echo "
// Fonctions de validation
function validateEmail(email) {
    return email.includes('@');
}

console.log('Fonctions de validation ajoutées');" >> utils.js
   ```

2. Ajoutez le fichier à la zone de staging et créez un commit :

   ```bash
   git add utils.js
   git commit -m "Ajout fonctions validation dans utils.js"
   ```

#### **4. Vérifier l'historique des commits :**

Maintenant que nous avons fait trois commits, tapez la commande suivante pour vérifier l’historique :

```bash
git log --oneline
```

Vous devriez voir un résultat similaire à ceci :

```
e7a4f8c Ajout fonctions validation dans utils.js
d2b9e5a Ajout styles boutons dans styles.css
c1f6b3d Ajout fonction multiply dans calculator.py
5a8c2e1 Initialisation du projet revert demo
```

**Représentation ASCII de l'historique avant revert :**
```
revert-test-branch
    ↓
    * e7a4f8c (HEAD) Ajout fonctions validation dans utils.js
    ↓
    * d2b9e5a Ajout styles boutons dans styles.css
    ↓
    * c1f6b3d Ajout fonction multiply dans calculator.py
    ↓
    * 5a8c2e1 (main) Initialisation du projet revert demo
```



### **Étape 4 : Utiliser `git revert` pour annuler un commit**

Nous allons maintenant annuler un des commits que nous avons faits.

**But :** Revenir en arrière et annuler le commit qui a modifié `styles.css` sans supprimer l'historique.

1. **Identifier l'ID du commit à annuler** :

   Regardez l'historique des commits que vous avez obtenus à l'étape précédente. Trouvez l'ID du commit qui a modifié `styles.css` (dans notre exemple, l'ID est `d2b9e5a`).

2. **Exécuter la commande `git revert`** :

   Pour annuler ce commit, exécutez la commande suivante en remplaçant `d2b9e5a` par l'ID de votre commit :

   ```bash
   git revert d2b9e5a
   ```

   Cette commande crée un nouveau commit qui inverse les changements apportés par le commit `d2b9e5a`.

3. **Modifier le message du commit (facultatif)** :

   Après avoir exécuté `git revert`, Git vous demandera de modifier le message du commit qui annule les changements. Vous pouvez laisser le message par défaut ou le personnaliser, puis enregistrez et fermez l'éditeur.

4. **Vérifier l’historique après le revert** :

   Tapez la commande suivante pour voir le nouvel historique des commits :

   ```bash
   git log --oneline
   ```

   Vous devriez voir quelque chose comme ceci :

   ```
   f9b2d8c Revert "Ajout styles boutons dans styles.css"
   e7a4f8c Ajout fonctions validation dans utils.js
   d2b9e5a Ajout styles boutons dans styles.css
   c1f6b3d Ajout fonction multiply dans calculator.py
   5a8c2e1 Initialisation du projet revert demo
   ```

   Le commit `f9b2d8c` est le nouveau commit créé par `git revert`, qui inverse les changements du commit `d2b9e5a`.

**Représentation ASCII de l'historique après revert :**
```
revert-test-branch
    ↓
    * f9b2d8c (HEAD) Revert "Ajout styles boutons dans styles.css"
    ↓
    * e7a4f8c Ajout fonctions validation dans utils.js
    ↓
    * d2b9e5a Ajout styles boutons dans styles.css  [ANNULÉ]
    ↓
    * c1f6b3d Ajout fonction multiply dans calculator.py
    ↓
    * 5a8c2e1 (main) Initialisation du projet revert demo
```

**Avantage du revert :**
- L'historique est conservé (pas de suppression de commits)
- Les changements sont "inversés" par un nouveau commit
- Plus sûr pour le travail collaboratif



### **Étape 5 : Vérifier les modifications dans le fichier `styles.css`**

Après avoir annulé le commit qui modifiait `styles.css`, nous allons vérifier que les modifications ont bien été annulées.

1. Ouvrez le fichier `styles.css` dans votre éditeur de texte.
   
   Vous devriez voir que les styles des boutons que vous aviez ajoutés :
   
   ```css
   /* Styles pour boutons */
   .button {
       padding: 10px 20px;
       background: #007bff;
       color: white;
       border: none;
   }
   ```

   ont été supprimés par le commit de revert.



### **Étape 6 : Finaliser l'exercice revert**

Après avoir expérimenté avec `git revert`, finalisez votre travail :

1. **Basculer vers la branche principale** :
   ```bash
   git checkout main
   ```

2. **Fusionner vos modifications** (si vous le souhaitez) :
   ```bash
   git merge revert-test-branch
   ```

3. **Nettoyer en supprimant la branche de test** :
   ```bash
   git branch -d revert-test-branch
   ```

**Représentation ASCII finale :**
```
main
  ↓
  * f9b2d8c Revert "Ajout styles boutons dans styles.css"
  ↓
  * e7a4f8c Ajout fonctions validation dans utils.js
  ↓
  * d2b9e5a Ajout styles boutons dans styles.css  [ANNULÉ]
  ↓
  * c1f6b3d Ajout fonction multiply dans calculator.py
  ↓
  * 5a8c2e1 Initialisation du projet revert demo
```

*Optionnel : Si vous travaillez avec un dépôt distant :*
```bash
git push origin main
```



## **Résumé des commandes**

### **Commandes complètes du tutoriel**

1. **Créer le projet exemple** :
   ```bash
   mkdir projet-revert-demo && cd projet-revert-demo
   git init
   echo "# Projet démonstration Git Revert" > README.md
   echo "def add(a, b): return a + b" > calculator.py
   echo "body { margin: 0; padding: 20px; }" > styles.css
   echo "console.log('Utilitaires chargés');" > utils.js
   git add . && git commit -m "Initialisation du projet revert demo"
   ```

2. **Créer une branche de test** :
   ```bash
   git checkout -b revert-test-branch
   ```

3. **Effectuer plusieurs commits** :
   ```bash
   # Commit 1
   echo "def multiply(a, b): return a * b" >> calculator.py
   git add calculator.py && git commit -m "Ajout fonction multiply dans calculator.py"
   
   # Commit 2  
   echo ".button { padding: 10px; background: #007bff; }" >> styles.css
   git add styles.css && git commit -m "Ajout styles boutons dans styles.css"
   
   # Commit 3
   echo "function validateEmail(email) { return email.includes('@'); }" >> utils.js
   git add utils.js && git commit -m "Ajout fonctions validation dans utils.js"
   ```

### **Commandes essentielles Git Revert**

4. **Annuler un commit spécifique avec `git revert`** :
   ```bash
   git revert <ID_du_commit>
   # Exemple: git revert d2b9e5a
   ```

5. **Vérifier l'historique après revert** :
   ```bash
   git log --oneline --graph
   ```

### **Commandes de finalisation**
```bash
git checkout main               # Retour branche principale
git merge revert-test-branch   # Fusionner si désiré
git branch -d revert-test-branch  # Supprimer branche test
```



## **Conclusion**

**Félicitations !** Vous avez appris à utiliser la commande **Git Revert** avec un projet exemple complet et autonome.

**Ce que vous avez appris :**
- Créer un projet Git multi-fichiers avec plusieurs commits
- Utiliser `git revert` pour annuler des commits spécifiques
- Comprendre la différence entre revert et reset
- Visualiser l'historique avant et après un revert

**Avantages de Git Revert :**
- **Historique préservé** : Aucun commit n'est supprimé
- **Traçabilité** : Les annulations sont visibles dans l'historique
- **Sécurité** : Idéal pour les environnements collaboratifs
- **Réversibilité** : Un revert peut lui-même être reverté

**Git Revert vs Git Reset :**
- **Revert** : Ajoute un nouveau commit qui annule les changements
- **Reset** : Supprime ou déplace des commits dans l'historique
- **Revert** : Plus sûr pour les branches partagées
- **Reset** : Plus dangereux, peut perdre des modifications

**Cas d'usage typiques :**
- Annuler un commit qui a introduit un bug
- Retirer une fonctionnalité problématique temporairement
- Revenir sur des changements en environnement collaboratif

Vous pouvez désormais utiliser Git Revert efficacement tout en préservant l'intégrité de l'historique de votre projet !


