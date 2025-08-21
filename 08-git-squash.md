
<a name="table-des-matieres"></a>

## Table des matières


1. [Introduction](#introduction)
2. [Partie 1 : Théorie](#partie-1-theorie)
   1. [Qu'est-ce que Git Squash ?](#qu-est-ce-que-git-squash)
   2. [Quand utiliser Git Squash ?](#quand-utiliser-git-squash)
   3. [Avantage de Git Squash](#avantage-de-git-squash)
3. [Partie 2 : Pratique](#partie-2-pratique)
   1. [Étape 1 : Cloner le projet existant](#etape-1)
   2. [Étape 2 : Créer une nouvelle branche et faire plusieurs commits](#etape-2)
   3. [Étape 3 : Combiner les commits avec Git Squash](#etape-3)
   4. [Étape 4 : Pousser la branche squashed vers GitHub](#etape-4)
4. [Résumé des commandes](#resume-commandes)
5. [Conclusion](#conclusion)


<br/>


<a name="introduction"></a>
# Introduction

      
Dans cette section, nous allons apprendre ce qu'est **Git Squash**, à quoi cela sert, et comment l'utiliser dans un projet. Ensuite, nous mettrons cela en pratique avec un projet exemple que nous créerons ensemble.

<br/>


<a name="partie-1-theorie"></a>
## 1 - **Partie 1 : Théorie**


<a name="qu-est-ce-que-git-squash"></a>
### 1. **Qu'est-ce que Git Squash ?**

Git Squash est une commande qui permet de combiner plusieurs commits en un seul. Cette commande est utile lorsqu’on souhaite simplifier l’historique Git ou regrouper plusieurs petits commits (comme des corrections ou des ajustements) en un seul commit plus propre et plus significatif. Cela est souvent fait avant de pousser les modifications vers un dépôt distant ou avant de fusionner une branche dans la branche principale.

<a name="quand-utiliser-git-squash"></a>
### 2. **Quand utiliser Git Squash ?**
- **Nettoyage de l’historique Git** : Si vous avez plusieurs commits mineurs (comme des corrections orthographiques ou des ajustements de code), vous pouvez les regrouper en un seul commit.
- **Avant une pull request** : Il est souvent recommandé de combiner plusieurs commits en un seul avant d’ouvrir une pull request pour garder l’historique propre.
- **Travail en équipe** : Git Squash est souvent utilisé pour simplifier l’historique lors de travaux collaboratifs, notamment avant de fusionner des branches.

<a name="avantage-de-git-squash"></a>
### 3. **Avantage de Git Squash :**
- Un historique plus propre et plus facile à lire.
- Meilleure organisation des commits (chaque commit ayant un sens plus global).

#### [⬆️ Retour à la table des matières](#table-des-matieres)
<br/>


<a name="partie-2-pratique"></a>
## 2 - **Partie 2 : Pratique**


<a name="etape-1"></a>
### **Étape 1 : Créer un nouveau projet exemple**

Nous allons créer un projet simple avec quelques fichiers pour démontrer Git Squash. Ouvrez votre terminal et créez un nouveau dossier de projet :

```bash
mkdir mon-projet-squash
cd mon-projet-squash
```

Ensuite, initialisons Git et créons la structure du projet :

```bash
git init
```

**Structure du projet :**
```
mon-projet-squash/
├── README.md
├── main.py  
├── config.txt
└── utils.js
```

Créons nos fichiers de base :

```bash
# Créer le fichier README.md
echo "# Mon Projet pour apprendre Git Squash" > README.md

# Créer un fichier Python simple
echo "print('Bonjour depuis main.py')" > main.py

# Créer un fichier de configuration
echo "# Configuration du projet" > config.txt

# Créer un fichier JavaScript utilitaire
echo "// Fonctions utilitaires" > utils.js
```

Ajoutons et commitons la structure initiale :

```bash
git add .
git commit -m "Structure initiale du projet"
```

#### [⬆️ Retour à la table des matières](#table-des-matieres)
<br/>


<a name="etape-2"></a>
### **Étape 2 : Créer une nouvelle branche et faire plusieurs commits**

Nous allons maintenant créer une nouvelle branche pour travailler dessus et faire plusieurs petits commits que nous combinerons ensuite avec Git Squash.

1. **Créer une nouvelle branche** :
   ```bash
   git checkout -b feature/ameliorations
   ```

2. **Effectuer plusieurs petites modifications** :
   
   - **Premier commit** : Améliorons le fichier `utils.js` :
   
   Ajoutons une fonction utilitaire au fichier `utils.js` :
   ```bash
   echo "
function saluer(nom) {
    return 'Bonjour ' + nom + '!';
}" >> utils.js
   ```
   
   Ensuite, ajoutons le fichier et créons un commit :
   ```bash
   git add utils.js
   git commit -m "Ajout fonction saluer dans utils.js"
   ```

   - **Deuxième commit** : Améliorons le fichier `main.py` :
   
   Ajoutons une fonction Python au fichier `main.py` :
   ```bash
   echo "
def calculer_somme(a, b):
    return a + b

print('Résultat:', calculer_somme(5, 3))" >> main.py
   ```

   Ensuite, ajoutons le fichier et créons un commit :
   ```bash
   git add main.py
   git commit -m "Ajout fonction calcul dans main.py"
   ```

   - **Troisième commit** : Mettons à jour la configuration :
   
   Ajoutons des paramètres dans `config.txt` :
   ```bash
   echo "
version=1.0.1
auteur=Mon Nom
debug=true" >> config.txt
   ```
   
   Et créons un commit :
   ```bash
   git add config.txt
   git commit -m "Mise à jour paramètres configuration"
   ```

**Visualisation de notre historique :**
```
* commit 3 - Mise à jour paramètres configuration
* commit 2 - Ajout fonction calcul dans main.py  
* commit 1 - Ajout fonction saluer dans utils.js
* commit 0 - Structure initiale du projet (main)
```

#### [⬆️ Retour à la table des matières](#table-des-matieres)
<br/>



<a name="etape-3"></a>
### **Étape 3 : Combiner les commits avec Git Squash**

Maintenant que nous avons fait plusieurs petits commits, nous allons les combiner en un seul commit pour avoir un historique plus propre.

1. **Voir l'historique des commits** :

   Avant de faire le squash, regardons l'historique des commits pour voir les trois derniers commits que nous avons créés :
   ```bash
   git log --oneline
   ```

   Vous devriez voir quelque chose comme ceci :
   ```
   7f8a9b2 Mise à jour paramètres configuration
   5c6d1e3 Ajout fonction calcul dans main.py
   2a3b4c5 Ajout fonction saluer dans utils.js
   1x2y3z4 Structure initiale du projet
   ```

**Représentation ASCII de l'historique AVANT squash :**
```
feature/ameliorations
    ↓
    * 7f8a9b2 (HEAD) Mise à jour paramètres configuration  
    ↓
    * 5c6d1e3 Ajout fonction calcul dans main.py
    ↓
    * 2a3b4c5 Ajout fonction saluer dans utils.js
    ↓
    * 1x2y3z4 (main) Structure initiale du projet
```

2. **Faire le squash des trois derniers commits** :

   Nous allons maintenant combiner ces trois commits en un seul. Pour cela, nous allons utiliser **git rebase** avec l'option **interactive**.

   Exécutez cette commande :
   ```bash
   git rebase -i HEAD~3
   ```

   Cette commande vous montrera les trois derniers commits. Vous allez voir une liste comme ceci dans votre éditeur :

   ```
   pick 2a3b4c5 Ajout fonction saluer dans utils.js
   pick 5c6d1e3 Ajout fonction calcul dans main.py
   pick 7f8a9b2 Mise à jour paramètres configuration
   ```

   **Modifier la liste pour faire un squash** :
   
   - Changez `pick` en `squash` ou simplement `s` pour les deux derniers commits, comme ceci :
     ```
     pick 2a3b4c5 Ajout fonction saluer dans utils.js
     squash 5c6d1e3 Ajout fonction calcul dans main.py
     squash 7f8a9b2 Mise à jour paramètres configuration
     ```

   - Enregistrez et fermez l'éditeur.

3. **Éditer le message du commit final** :

   Après avoir fermé l'éditeur, Git vous demandera de modifier le message du nouveau commit combiné. Vous verrez les messages des trois commits précédents, que vous pouvez modifier ou combiner.

   Exemple de message combiné :
   ```
   Amélioration du projet avec nouvelles fonctionnalités
   
   - Ajout fonction saluer dans utils.js
   - Ajout fonction calcul dans main.py  
   - Mise à jour paramètres configuration
   ```

   Enregistrez et fermez l'éditeur pour finaliser le squash.

4. **Vérifier l'historique après le squash** :

   Une fois le squash effectué, vérifiez à nouveau l'historique des commits avec la commande suivante :

   ```bash
   git log --oneline
   ```

   Vous devriez maintenant voir un seul commit qui combine les trois précédents :
   ```
   9e8f7a6 Amélioration du projet avec nouvelles fonctionnalités  
   1x2y3z4 Structure initiale du projet
   ```

**Représentation ASCII de l'historique APRÈS squash :**
```
feature/ameliorations
    ↓
    * 9e8f7a6 (HEAD) Amélioration du projet avec nouvelles fonctionnalités
    ↓
    * 1x2y3z4 (main) Structure initiale du projet
```

**Comparaison AVANT/APRÈS :**
```
AVANT squash:                    APRÈS squash:
                  --
* Mise à jour config      ═══>   * Amélioration du projet 
* Ajout fonction calcul   ═══>     avec nouvelles fonctionnalités
* Ajout fonction saluer   ═══>   
* Structure initiale             * Structure initiale
```

**Avantages observés :**
- Historique plus propre et lisible  
- Un seul commit décrivant l'ensemble des améliorations
- Plus facile pour les futures révisions de code

#### [⬆️ Retour à la table des matières](#table-des-matieres)
<br/>


<a name="etape-4"></a>
### **Étape 4 : Finaliser et intégrer les changements**

Maintenant que nous avons effectué le squash, nous pouvons finaliser notre travail.

1. **Basculer vers la branche principale et merger** :
   ```bash
   git checkout main
   git merge feature/ameliorations
   ```

2. **Vérifier le résultat final** :
   ```bash
   git log --oneline --graph
   ```

   Vous verrez maintenant un historique propre :
   ```
   * 9e8f7a6 (HEAD -> main, feature/ameliorations) Amélioration du projet avec nouvelles fonctionnalités
   * 1x2y3z4 Structure initiale du projet
   ```

3. **Nettoyer en supprimant la branche de feature** :
   ```bash
   git branch -d feature/ameliorations
   ```

**Structure finale du projet :**
```
mon-projet-squash/
├── README.md            (contenu initial)
├── main.py             (+ fonction calculer_somme)
├── config.txt          (+ paramètres de config)  
└── utils.js            (+ fonction saluer)
```

*Optionnel : Si vous travaillez avec un dépôt distant, vous pouvez pousser vers GitHub/GitLab :*
```bash
git push origin main
```

#### [⬆️ Retour à la table des matières](#table-des-matieres)
<br/>



<a name="resume-commandes"></a>
## **Résumé des commandes**

### **Commandes complètes du tutoriel**

1. **Créer le projet exemple** :
   ```bash
   mkdir mon-projet-squash && cd mon-projet-squash
   git init
   echo "# Mon Projet pour apprendre Git Squash" > README.md
   echo "print('Bonjour depuis main.py')" > main.py
   echo "# Configuration du projet" > config.txt
   echo "// Fonctions utilitaires" > utils.js
   git add . && git commit -m "Structure initiale du projet"
   ```

2. **Créer une branche et faire plusieurs commits** :
   ```bash
   git checkout -b feature/ameliorations
   ```
   
   ```bash
   # Commit 1
   echo "function saluer(nom) { return 'Bonjour ' + nom + '!'; }" >> utils.js
   git add utils.js && git commit -m "Ajout fonction saluer dans utils.js"
   
   # Commit 2  
   echo "def calculer_somme(a, b): return a + b" >> main.py
   git add main.py && git commit -m "Ajout fonction calcul dans main.py"
   
   # Commit 3
   echo -e "version=1.0.1\nauteur=Mon Nom\ndebug=true" >> config.txt
   git add config.txt && git commit -m "Mise à jour paramètres configuration"
   ```

3. **Faire un Git Squash** :
   ```bash
   git rebase -i HEAD~3
   # Changer 'pick' en 'squash' pour les 2 derniers commits
   # Modifier le message de commit combiné
   ```

4. **Finaliser et intégrer** :
   ```bash
   git checkout main
   git merge feature/ameliorations
   git branch -d feature/ameliorations
   ```

### **Commandes essentielles Git Squash**
```bash
git log --oneline           # Voir l'historique
git rebase -i HEAD~N        # Squash N derniers commits  
git log --oneline --graph   # Visualiser l'historique
```

#### [⬆️ Retour à la table des matières](#table-des-matieres)
<br/>




<a name="conclusion"></a>
### 3 - **Conclusion**

**Félicitations !** Ce guide vous a montré comment utiliser **Git Squash** pour combiner plusieurs commits en un seul, avec un projet exemple complet et autonome.

**Ce que vous avez appris :**
- Créer un projet Git depuis zéro
- Faire plusieurs commits sur une branche de feature  
- Utiliser `git rebase -i` pour combiner des commits
- Comprendre l'impact du squash sur l'historique Git
- Visualiser les changements avec des diagrammes ASCII

**Bonnes pratiques retenues :**
- **Historique propre** : Un commit par fonctionnalité/amélioration
- **Messages clairs** : Descriptions détaillées des changements
- **Branches organisées** : Travail isolé avant intégration
- **Squash intelligent** : Regrouper les commits liés

**Prochaines étapes suggérées :**
- Pratiquer sur vos propres projets
- Explorer `git rebase` interactif pour d'autres cas d'usage
- Apprendre les différences entre `merge` et `rebase`

Vous pouvez désormais utiliser cette technique pour maintenir un historique Git professionnel et bien organisé !

#### [⬆️ Retour à la table des matières](#table-des-matieres)
<br/>



