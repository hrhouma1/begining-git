
<a name="table-des-matieres"></a>

## Table des matières


### **Objectif :**
Ce guide vous apprendra à gérer les **branches distantes** dans Git. Vous apprendrez comment créer, pousser, récupérer et gérer des branches distantes dans GitHub et synchroniser votre travail avec vos collaborateurs.



## **Partie 1 : Théorie**

### **Qu'est-ce qu'une branche distante ?**

- **Une branche distante** est une branche qui existe dans un dépôt distant, comme GitHub, mais pas forcément dans votre dépôt local. Les branches distantes vous permettent de collaborer avec d'autres personnes sur un projet, car elles sont partagées et accessibles par tous les contributeurs.

### **Pourquoi utiliser des branches distantes ?**

- **Collaboration** : Les branches distantes permettent à plusieurs personnes de travailler sur différentes fonctionnalités sans interférer avec la branche principale. Vous pouvez pousser vos branches vers GitHub pour les partager, puis les fusionner une fois le travail terminé.
- **Gestion du code** : Cela permet de maintenir une branche stable (souvent appelée `main` ou `master`) et de travailler sur des branches de développement ou de fonctionnalités sans perturber la branche principale.

### **Les commandes principales pour les branches distantes :**

1. **`git push`** : Pousser une branche locale vers le dépôt distant.
2. **`git fetch`** : Récupérer les branches distantes sans les fusionner avec vos branches locales.
3. **`git pull`** : Récupérer et fusionner les branches distantes avec vos branches locales.
4. **`git branch -r`** : Voir toutes les branches distantes.
5. **`git checkout -b`** : Créer une branche locale à partir d'une branche distante.



## **Partie 2 : Pratique**

Nous allons maintenant manipuler des branches distantes avec un projet **générique**. Vous apprendrez à créer, récupérer et gérer des branches distantes avec GitHub.

### **Étape 1 : Créer un projet pour les branches distantes**

Créons un projet d'application web simple pour démontrer la gestion des branches distantes :

```bash
mkdir projet-branches-distantes
cd projet-branches-distantes
git init
```

**Structure du projet :**
```
projet-branches-distantes/
├── README.md
├── index.html
├── styles.css
├── app.js
└── config.json
```

**Initialisation du projet :**

```bash
# Créer les fichiers de base
echo "# Projet Branches Distantes
Application web pour démontrer la gestion des branches distantes Git.

## Fonctionnalités
- Interface utilisateur interactive
- Gestion des styles CSS
- JavaScript modulaire
- Configuration JSON" > README.md

echo "<!DOCTYPE html>
<html lang='fr'>
<head>
    <meta charset='UTF-8'>
    <title>Gestion Branches Distantes</title>
    <link rel='stylesheet' href='styles.css'>
</head>
<body>
    <header>
        <h1>Application Branches Distantes</h1>
    </header>
    <main>
        <div class='container'>
            <h2>Démonstration Git Remote Branches</h2>
            <p>Projet pour apprendre la gestion des branches distantes</p>
        </div>
    </main>
    <script src='app.js'></script>
</body>
</html>" > index.html

echo "/* Styles de base pour l'application */
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    background-color: #f5f5f5;
}

header {
    background-color: #2c3e50;
    color: white;
    text-align: center;
    padding: 1rem 0;
}

.container {
    max-width: 800px;
    margin: 2rem auto;
    padding: 2rem;
    background: white;
    border-radius: 8px;
    box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}" > styles.css

echo "// Application principale
console.log('Application branches distantes initialisée');

document.addEventListener('DOMContentLoaded', function() {
    console.log('DOM chargé - Version initiale');
    
    // Initialiser l'interface
    initializeApp();
});

function initializeApp() {
    console.log('Initialisation de l\'application');
    
    // Ajouter des écouteurs d'événements
    const header = document.querySelector('h1');
    if (header) {
        header.addEventListener('click', function() {
            console.log('Header cliqué');
        });
    }
}" > app.js

echo '{
  "name": "projet-branches-distantes",
  "version": "1.0.0",
  "description": "Projet pour démontrer les branches distantes Git",
  "main": "app.js",
  "scripts": {
    "start": "python -m http.server 8000"
  }
}' > config.json

# Premier commit
git add .
git commit -m "Version initiale du projet branches distantes"
```

**Connecter à GitHub :**
```bash
# Créer un nouveau dépôt sur GitHub puis :
git remote add origin https://github.com/VOTRE_USERNAME/projet-branches-distantes.git
git push -u origin main
```



### **Étape 2 : Créer une nouvelle branche locale et la pousser vers GitHub**

1. **Créer une nouvelle branche** :

   Nous allons créer une nouvelle branche locale appelée `feature/interface-amelioree` pour travailler dessus :

   ```bash
   git checkout -b feature/interface-amelioree
   ```

2. **Améliorer l'interface dans `app.js`** :

   Ouvrez le fichier `app.js` et remplacez son contenu par :

   ```javascript
   // Application principale - Interface améliorée
   console.log('Application branches distantes - Version améliorée');

   document.addEventListener('DOMContentLoaded', function() {
       console.log('DOM chargé - Interface améliorée');
       
       // Initialiser l'interface améliorée
       initializeEnhancedApp();
   });

   function initializeEnhancedApp() {
       console.log('Initialisation de l\'interface améliorée');
       
       // Ajouter des fonctionnalités interactives
       addInteractiveFeatures();
       
       // Afficher un message de bienvenue
       showWelcomeMessage();
   }

   function addInteractiveFeatures() {
       const header = document.querySelector('h1');
       const container = document.querySelector('.container');
       
       if (header) {
           header.addEventListener('click', function() {
               header.style.color = header.style.color === 'gold' ? 'white' : 'gold';
               console.log('Couleur du header modifiée');
           });
       }
       
       if (container) {
           container.addEventListener('mouseenter', function() {
               this.style.transform = 'scale(1.02)';
               this.style.transition = 'transform 0.3s ease';
           });
           
           container.addEventListener('mouseleave', function() {
               this.style.transform = 'scale(1)';
           });
       }
   }

   function showWelcomeMessage() {
       setTimeout(function() {
           console.log('Bienvenue dans l\'interface améliorée !');
       }, 1000);
   }
   ```

3. **Améliorer également les styles :**

   ```bash
   echo "/* Styles améliorés pour l'application */
   body {
       font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
       margin: 0;
       padding: 0;
       background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
       min-height: 100vh;
   }

   header {
       background: rgba(44, 62, 80, 0.9);
       color: white;
       text-align: center;
       padding: 1.5rem 0;
       backdrop-filter: blur(10px);
   }

   header h1 {
       cursor: pointer;
       transition: color 0.3s ease;
       margin: 0;
   }

   .container {
       max-width: 800px;
       margin: 2rem auto;
       padding: 2rem;
       background: rgba(255, 255, 255, 0.95);
       border-radius: 12px;
       box-shadow: 0 8px 32px rgba(0,0,0,0.1);
       backdrop-filter: blur(10px);
       cursor: pointer;
   }

   .container:hover {
       box-shadow: 0 12px 48px rgba(0,0,0,0.15);
   }" > styles.css
   ```

4. **Ajouter et committer les modifications** :

   - Ajoutez les fichiers modifiés à la zone de staging :
   
     ```bash
     git add app.js styles.css
     ```

   - Créez un commit pour cette modification :
   
     ```bash
     git commit -m "Amélioration interface: ajout interactivité et styles modernisés"
     ```

5. **Pousser la branche vers GitHub** :

   Maintenant que nous avons fait des modifications dans la branche locale, nous allons pousser cette branche vers le dépôt GitHub pour la rendre accessible à d'autres collaborateurs :

   ```bash
   git push -u origin feature/interface-amelioree
   ```

   - **`-u`** : Cela permet de lier la branche locale à la branche distante.
   - **`origin`** : Il s'agit du dépôt distant (par défaut, le nom est `origin`).
   - **`feature/interface-amelioree`** : Il s'agit du nom de la branche locale que nous poussons vers GitHub.



### **Étape 3 : Voir toutes les branches distantes**

Pour voir toutes les branches distantes disponibles dans le dépôt GitHub, vous pouvez utiliser la commande suivante :

```bash
git branch -r
```

Cette commande liste toutes les branches distantes du dépôt GitHub. Vous verrez quelque chose comme ceci :

```
  origin/main
  origin/feature/interface-amelioree
```

Cela signifie que `main` et `feature/interface-amelioree` sont des branches distantes disponibles sur GitHub.

**Visualisation des branches :**
```
Branches locales et distantes :
  
  main (local)
  ↓
  * Version initiale du projet branches distantes
  
  feature/interface-amelioree (local + remote)
  ↓  
  * Amélioration interface: ajout interactivité et styles modernisés
  ↓
  * Version initiale du projet branches distantes
```



### **Étape 4 : Récupérer une branche distante avec `git fetch`**

Supposons qu'un collaborateur a créé une nouvelle branche sur GitHub et que vous voulez la récupérer sans fusionner les modifications directement dans votre branche locale.

1. **Récupérer les branches distantes sans les fusionner** :

   Tapez la commande suivante pour récupérer toutes les branches distantes :

   ```bash
   git fetch
   ```

   Cette commande va récupérer les informations sur les branches distantes, mais elle ne fusionnera pas les modifications dans votre branche locale.

2. **Voir les branches locales et distantes après un `fetch`** :

   Après avoir utilisé `git fetch`, tapez la commande suivante pour voir toutes les branches locales et distantes disponibles :

   ```bash
   git branch -a
   ```

   Vous verrez une liste comme celle-ci :

   ```
   * main
     feature/interface-amelioree
     remotes/origin/main
     remotes/origin/feature/interface-amelioree
   ```

   **Explication :**
   - Les branches **sans préfixe** sont les branches locales
   - Les branches **`remotes/origin/`** sont les branches distantes sur GitHub
   - L'**astérisque (*)** indique la branche courante



### **Étape 5 : Travailler avec une branche distante**

Si vous souhaitez travailler sur une branche distante (par exemple `feature/interface-amelioree`), vous pouvez créer une copie locale de cette branche.

**Scénario :** Un collaborateur a créé une branche `feature/documentation` que vous voulez récupérer.

1. **Créer une nouvelle branche locale à partir d'une branche distante** :

   Si la branche distante `feature/documentation` n'existe pas localement, vous pouvez la récupérer et basculer dessus avec la commande suivante :

   ```bash
   git fetch  # D'abord récupérer les dernières branches distantes
   git checkout -b feature/documentation origin/feature/documentation
   ```

   Cela crée une nouvelle branche locale basée sur la branche distante `feature/documentation` et vous permet de travailler dessus.

2. **Créer une branche de documentation (exemple pratique)** :

   Créons une branche pour ajouter de la documentation :

   ```bash
   git checkout -b feature/documentation
   
   # Ajouter un fichier de documentation
   echo "# Documentation du Projet

   ## Architecture
   Le projet est structuré comme suit :
   - **index.html** : Page principale
   - **styles.css** : Feuilles de style
   - **app.js** : Logique JavaScript
   - **config.json** : Configuration

   ## Installation
   1. Cloner le dépôt
   2. Ouvrir index.html dans un navigateur
   3. Ou utiliser un serveur local : python -m http.server 8000

   ## Développement
   Pour contribuer au projet :
   1. Créer une branche feature
   2. Développer la fonctionnalité
   3. Tester les modifications
   4. Créer une pull request
   " > DOCUMENTATION.md
   
   # Committer la documentation
   git add DOCUMENTATION.md
   git commit -m "Ajout documentation du projet"
   
   # Pousser vers GitHub
   git push -u origin feature/documentation
   ```

3. **Faire d'autres modifications et pousser les changements** :

   - Continuez à faire des modifications dans cette branche
   - Ajoutez les fichiers modifiés à la zone de staging :

     ```bash
     git add DOCUMENTATION.md
     ```

   - Créez un commit :

     ```bash
     git commit -m "Amélioration de la documentation avec exemples"
     ```

   - Poussez la branche locale modifiée vers GitHub :

     ```bash
     git push origin feature/documentation
     ```



### **Étape 6 : Fusionner une branche distante dans `main`**

Après avoir travaillé sur une branche distante, vous pouvez vouloir fusionner cette branche dans la branche principale (`main`).

1. **Basculer sur la branche `main`** :

   ```bash
   git checkout main
   ```

2. **Récupérer les dernières modifications de `main` depuis GitHub** :

   Avant de fusionner quoi que ce soit, assurez-vous d'avoir les dernières modifications de la branche `main` depuis GitHub :

   ```bash
   git pull origin main
   ```

3. **Fusionner la branche de documentation dans `main`** :

   Fusionnez ensuite la branche distante (qui a été récupérée localement) dans `main` :

   ```bash
   git merge feature/documentation
   ```

4. **Pousser les modifications de `main` vers GitHub** :

   Après la fusion, poussez les changements dans `main` vers GitHub :

   ```bash
   git push origin main
   ```

**Visualisation après fusion :**
```
main (HEAD, local + remote)
  ↓
  *   Merge branch 'feature/documentation' into main
  |\
  | * Amélioration de la documentation avec exemples  
  | * Ajout documentation du projet
  |/
  * Version initiale du projet branches distantes
```



### **Étape 7 : Supprimer une branche distante**

Une fois que vous avez fusionné une branche et qu'elle n'est plus nécessaire, vous pouvez la supprimer de GitHub.

1. **Supprimer une branche distante** :

   Pour supprimer une branche distante sur GitHub, utilisez la commande suivante :

   ```bash
   git push origin --delete feature/documentation
   ```

**Nettoyage complet :**
```bash
git branch -d feature/documentation     # Supprimer localement
git fetch --prune                       # Nettoyer les références
```

Cela supprimera la branche `feature/documentation` du dépôt GitHub et nettoiera votre environnement local.



### **Étape 8 : Résumé des commandes pour gérer les branches distantes**

1. **Créer une nouvelle branche et la pousser vers GitHub** :
   ```bash
   git checkout -b feature/nouvelle-fonctionnalite
   git push -u origin feature/nouvelle-fonctionnalite
   ```

2. **Voir toutes les branches distantes** :
   ```bash
   git branch -r
   ```

3. **Récupérer les branches distantes sans les fusionner** :
   ```bash
   git fetch
   ```

4. **Créer une branche locale à partir d'une branche distante** :
   ```bash
   git checkout -b feature/documentation origin/feature/documentation
   ```

5. **Fusionner une branche distante dans `main`** :
  




-
### **Étape 8 : Résumé des commandes pour gérer les branches distantes** (suite)
-

5. **Fusionner une branche distante dans `main`** :
   ```bash
   git checkout main
   git pull origin main
   git merge feature/documentation
   git push origin main
   ```

6. **Supprimer une branche distante sur GitHub** :
   ```bash
   git push origin --delete feature/documentation
   git branch -d feature/documentation     # Aussi localement
   git fetch --prune                       # Nettoyer les références
   ```



### **Conclusion**

**Félicitations !** Vous maîtrisez maintenant la gestion des **branches distantes** dans Git.

**Ce que vous avez appris :**
- Créer et pousser des branches vers GitHub
- Récupérer et travailler avec des branches distantes 
- Fusionner des branches dans la branche principale
- Nettoyer et supprimer des branches obsolètes
- Gérer la collaboration avec plusieurs développeurs

**Points clés à retenir :**
- **`git fetch`** récupère sans fusionner (sécurisé)
- **`git pull`** récupère et fusionne automatiquement  
- **`-u`** établit le lien entre branches locales et distantes
- **`--prune`** nettoie les références aux branches supprimées
- Les branches distantes permettent une collaboration structurée

**Workflow recommandé :**
1. **Fetch** régulièrement pour rester à jour
2. **Créer des branches** pour chaque fonctionnalité
3. **Pousser** vos branches pour partager le travail
4. **Fusionner** via pull requests (recommandé) ou en local
5. **Nettoyer** les branches après fusion

Les branches distantes sont le **fondement de la collaboration** moderne avec Git. Elles permettent à plusieurs développeurs de travailler en parallèle tout en maintenant un historique propre et traçable.

Vous êtes désormais équipé pour collaborer efficacement sur des projets distribués avec GitHub !
