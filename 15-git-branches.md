---
title: "Chapitre 15 - Branches Git"
description: "Découvrez comment utiliser les branches dans Git pour travailler sur différentes fonctionnalités en parallèle"
---

---
<a name="table-des-matieres"></a>

## Table des matières
---

### **Objectif :**
Ce guide vous apprendra à utiliser les branches dans Git. Les branches sont un élément essentiel de Git qui vous permettent de travailler sur des fonctionnalités distinctes sans affecter la branche principale de votre projet. Vous apprendrez à créer, changer, fusionner, et supprimer des branches, ainsi qu'à gérer plusieurs branches dans un projet Git.

---

## **Partie 1 : Théorie**

### **Qu'est-ce qu'une branche Git ?**

- **Une branche** est une version parallèle de votre projet. Par défaut, Git utilise la branche principale appelée **`main`**. Vous pouvez créer plusieurs branches pour travailler sur différentes fonctionnalités, puis fusionner ces branches dans `main` une fois que votre travail est terminé et testé.

### **Pourquoi utiliser les branches ?**
- **Travail en parallèle** : Vous pouvez créer une branche pour chaque nouvelle fonctionnalité ou correction de bug sans affecter la version stable de votre projet.
- **Collaboration** : Chaque développeur peut travailler sur sa propre branche, puis fusionner son travail dans `main` ou une autre branche lorsque tout est prêt.
- **Expérimentations** : Les branches permettent d'essayer des idées sans toucher au projet principal.

---

## **Partie 2 : Pratique**

Nous allons créer un projet exemple et manipuler plusieurs branches avec des exemples concrets.

### **Étape 1 : Créer un projet exemple pour les branches**

Créons un projet complet pour démontrer le travail avec les branches Git.

```bash
mkdir projet-branches-demo
cd projet-branches-demo
git init
```

**Structure du projet :**
```
projet-branches-demo/
├── README.md
├── app.py
├── database.py
├── auth.py
├── frontend/
│   ├── index.html
│   └── styles.css
└── tests/
    └── test_app.py
```

Créons notre projet de base :

```bash
# Créer la structure
mkdir frontend tests

# Fichiers principaux
echo "# Application Multi-Branches
Application de démonstration des branches Git.

## Fonctionnalités
- Application de base
- Authentification
- Base de données
- Interface frontend" > README.md

echo "#!/usr/bin/env python3
class Application:
    def __init__(self):
        self.version = '1.0.0'
        self.name = 'App Branches Demo'
    
    def start(self):
        print(f'Démarrage de {self.name} v{self.version}')
        return True
    
    def stop(self):
        print('Arrêt de l application')
        return True

if __name__ == '__main__':
    app = Application()
    app.start()" > app.py

echo "# Gestionnaire de base de données
class Database:
    def __init__(self):
        self.connected = False
    
    def connect(self):
        print('Connexion à la base de données')
        self.connected = True" > database.py

echo "<!DOCTYPE html>
<html>
<head>
    <title>App Demo</title>
    <link rel='stylesheet' href='styles.css'>
</head>
<body>
    <h1>Application de démonstration</h1>
    <p>Interface utilisateur de base</p>
</body>
</html>" > frontend/index.html

echo "body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 20px;
    background-color: #f5f5f5;
}

h1 {
    color: #333;
    text-align: center;
}" > frontend/styles.css

echo "import unittest

class TestApp(unittest.TestCase):
    def test_app_creation(self):
        # Test de base
        self.assertTrue(True)

if __name__ == '__main__':
    unittest.main()" > tests/test_app.py

# Commit initial
git add .
git commit -m "Structure initiale de l'application avec modules de base"
```

---

### **Étape 2 : Créer une nouvelle branche**

Nous allons maintenant créer une nouvelle branche pour travailler sur une nouvelle fonctionnalité, sans toucher à la branche principale.

1. **Créer une nouvelle branche pour l'authentification** :

   ```bash
   git checkout -b feature/authentification
   ```

   - **`checkout -b`** : Crée et bascule sur la nouvelle branche
   - Nous travaillons maintenant sur `feature/authentification`

2. **Vérifier sur quelle branche vous êtes** :

   ```bash
   git branch
   ```

   Résultat :
   ```
   * feature/authentification
     main
   ```

**Représentation ASCII des branches :**
```
main
  ↓
  * Structure initiale
  │
  └── feature/authentification (HEAD)
      * (prêt pour développement)
```

---

### **Étape 3 : Faire des modifications dans la nouvelle branche**

Développons la fonctionnalité d'authentification sur cette branche.

#### **1. Développer le module d'authentification :**

```bash
echo "#!/usr/bin/env python3
class AuthManager:
    def __init__(self):
        self.users = {}
        self.current_user = None
    
    def register_user(self, username, password):
        \"\"\"Enregistrer un nouvel utilisateur\"\"\"
        if username not in self.users:
            self.users[username] = password
            print(f'Utilisateur {username} enregistré')
            return True
        print('Utilisateur déjà existant')
        return False
    
    def login(self, username, password):
        \"\"\"Connecter un utilisateur\"\"\"
        if username in self.users and self.users[username] == password:
            self.current_user = username
            print(f'Connexion réussie pour {username}')
            return True
        print('Échec de la connexion')
        return False
    
    def logout(self):
        \"\"\"Déconnecter l'utilisateur actuel\"\"\"
        if self.current_user:
            print(f'Déconnexion de {self.current_user}')
            self.current_user = None
            return True
        return False
        
    def is_authenticated(self):
        \"\"\"Vérifier si un utilisateur est connecté\"\"\"
        return self.current_user is not None

if __name__ == '__main__':
    auth = AuthManager()
    auth.register_user('admin', 'password123')
    auth.login('admin', 'password123')
    print(f'Authentifié: {auth.is_authenticated()}')" > auth.py

git add auth.py
git commit -m "Ajout module authentification complet avec gestion utilisateurs"
```

#### **2. Ajouter des tests pour l'authentification :**

```bash
echo "#!/usr/bin/env python3
import unittest
import sys
import os
sys.path.append(os.path.dirname(os.path.dirname(__file__)))

from auth import AuthManager

class TestAuthManager(unittest.TestCase):
    def setUp(self):
        self.auth = AuthManager()
    
    def test_register_user_success(self):
        result = self.auth.register_user('testuser', 'testpass')
        self.assertTrue(result)
        
    def test_register_duplicate_user(self):
        self.auth.register_user('testuser', 'testpass')
        result = self.auth.register_user('testuser', 'newpass')
        self.assertFalse(result)
        
    def test_login_success(self):
        self.auth.register_user('testuser', 'testpass')
        result = self.auth.login('testuser', 'testpass')
        self.assertTrue(result)
        self.assertTrue(self.auth.is_authenticated())
        
    def test_login_failure(self):
        result = self.auth.login('nonexistent', 'wrongpass')
        self.assertFalse(result)
        self.assertFalse(self.auth.is_authenticated())
        
    def test_logout(self):
        self.auth.register_user('testuser', 'testpass')
        self.auth.login('testuser', 'testpass')
        result = self.auth.logout()
        self.assertTrue(result)
        self.assertFalse(self.auth.is_authenticated())

if __name__ == '__main__':
    unittest.main()" > tests/test_auth_complete.py

git add tests/test_auth_complete.py
git commit -m "Ajout tests unitaires complets pour authentification"
```

---

### **Étape 4 : Créer plusieurs branches en parallèle**

Créons rapidement d'autres branches pour différentes fonctionnalités.

#### **1. Branche pour l'interface frontend :**

```bash
git checkout main  # Retour à main
git checkout -b feature/frontend-ameliore

# Améliorer l'interface
echo "<!DOCTYPE html>
<html>
<head>
    <title>App Demo - Version Améliorée</title>
    <link rel='stylesheet' href='styles.css'>
</head>
<body>
    <header>
        <h1>Application Multi-Branches</h1>
        <nav>
            <button onclick='login()'>Connexion</button>
            <button onclick='logout()'>Déconnexion</button>
        </nav>
    </header>
    <main>
        <section id='content'>
            <p>Interface utilisateur améliorée avec navigation</p>
            <div id='user-status'>Non connecté</div>
        </section>
    </main>
    <script>
        function login() { alert('Fonctionnalité de connexion'); }
        function logout() { alert('Déconnexion'); }
    </script>
</body>
</html>" > frontend/index.html

git add frontend/index.html
git commit -m "Interface frontend améliorée avec navigation"
```

#### **2. Branche pour la base de données :**

```bash
git checkout main  # Retour à main
git checkout -b feature/database-avancee

# Améliorer la base de données
echo "#!/usr/bin/env python3
import json
from datetime import datetime

class DatabaseManager:
    def __init__(self, db_file='data.json'):
        self.db_file = db_file
        self.connected = False
        
    def connect(self):
        print('Connexion à la base de données avancée')
        self.connected = True
        return self.connected
    
    def save_data(self, table, data):
        if not self.connected:
            return False
        # Simulation sauvegarde
        print(f'Sauvegarde dans {table}: {data}')
        return True
        
    def load_data(self, table):
        if not self.connected:
            return None
        print(f'Chargement depuis {table}')
        return {'status': 'loaded', 'timestamp': str(datetime.now())}

if __name__ == '__main__':
    db = DatabaseManager()
    db.connect()
    db.save_data('users', {'name': 'test'})
    print(db.load_data('users'))" > database.py

git add database.py
git commit -m "Base de données avancée avec gestion JSON et timestamps"
```

**Visualisation des branches :**
```
main
├── feature/authentification
│   ├── auth.py
│   └── tests/test_auth_complete.py
├── feature/frontend-ameliore  
│   └── frontend/index.html (amélioré)
└── feature/database-avancee
    └── database.py (amélioré)
```

---

### **Étape 5 : Fusionner les branches dans main**

Intégrons les fonctionnalités développées dans la branche principale.

```bash
# Retour sur main
git checkout main

# Fusionner l'authentification
git merge feature/authentification

# Fusionner le frontend amélioré  
git merge feature/frontend-ameliore

# Fusionner la base de données avancée
git merge feature/database-avancee

# Vérifier le résultat
git log --oneline --graph
```

**Représentation ASCII après fusion :**
```
main (après fusions)
├─ * Fusion feature/database-avancee
├─ * Fusion feature/frontend-ameliore  
├─ * Fusion feature/authentification
└─ * Structure initiale de l'application
```

**État final du projet :**
```
projet-branches-demo/
├── README.md
├── app.py              (original)
├── database.py         (amélioré avec JSON)
├── auth.py            (nouveau module complet)
├── frontend/
│   ├── index.html     (interface améliorée)
│   └── styles.css     (original)
└── tests/
    ├── test_app.py    (original)  
    └── test_auth_complete.py (nouveaux tests)
```

---

### **Étape 6 : Nettoyer les branches**

Après fusion, supprimons les branches devenues inutiles :

```bash
# Supprimer les branches fusionnées
git branch -d feature/authentification
git branch -d feature/frontend-ameliore  
git branch -d feature/database-avancee

# Vérifier qu'il ne reste que main
git branch
```

---

## **Résumé des commandes**

### **Commandes complètes du tutoriel**

1. **Créer le projet avec branches** :
   ```bash
   mkdir projet-branches-demo && cd projet-branches-demo
   git init
   # ... créer structure projet ...
   git add . && git commit -m "Structure initiale"
   ```

2. **Gestion des branches** :
   ```bash
   git checkout -b feature/nom-feature    # Créer + basculer
   # ... développement ...
   git add . && git commit -m "Fonctionnalité"
   git checkout main                      # Retour main
   git merge feature/nom-feature          # Fusionner
   git branch -d feature/nom-feature      # Supprimer
   ```

### **Commandes essentielles branches Git**

```bash
# Gestion branches
git branch                        # Lister branches
git checkout -b nouvelle-branche  # Créer + basculer
git checkout branche-existante    # Basculer
git branch -d branche            # Supprimer (fusionnée)
git branch -D branche            # Supprimer (force)

# Fusion
git merge nom-branche            # Fusionner branche dans actuelle
git log --oneline --graph        # Voir historique graphique

# Branches distantes  
git push -u origin nom-branche   # Pousser nouvelle branche
git push origin main            # Pousser main
```

### **Workflow branches recommandé**
```bash
1. git checkout main                    # Sur main
2. git pull origin main                # Mise à jour
3. git checkout -b feature/ma-feature  # Nouvelle branche
4. # ... développement et commits ...
5. git checkout main                    # Retour main
6. git merge feature/ma-feature        # Fusion
7. git branch -d feature/ma-feature    # Nettoyage
8. git push origin main                # Push résultat
```

---

## **Conclusion**

**Félicitations !** Vous avez maîtrisé les **branches Git** avec un projet exemple complet et autonome.

**Ce que vous avez appris :**
- Créer un projet multi-modules avec architecture claire
- Utiliser les branches pour le développement parallèle de fonctionnalités
- Fusionner les branches et intégrer les fonctionnalités
- Gérer un workflow de développement professionnel
- Visualiser l'historique des branches avec ASCII

**Concepts clés des branches :**
- **Isolation** : Chaque fonctionnalité sur sa propre branche
- **Parallélisme** : Plusieurs développeurs/fonctionnalités simultanément
- **Integration** : Fusion contrôlée dans la branche principale
- **Nettoyage** : Suppression des branches après fusion

**Types de branches courantes :**
- **main/master** : Branche principale stable
- **feature/*** : Nouvelles fonctionnalités
- **bugfix/*** : Corrections de bugs
- **hotfix/*** : Corrections urgentes en production
- **develop** : Branche de développement intégré

**Avantages des branches :**
- **Sécurité** : main reste toujours stable
- **Collaboration** : Chacun travaille sur sa branche
- **Expérimentation** : Test de nouvelles idées sans risque
- **Organisation** : Historique clair par fonctionnalité
- **Revue** : Contrôle qualité avant intégration

**Stratégies de branches populaires :**
- **Git Flow** : main + develop + features + hotfix
- **GitHub Flow** : main + features (plus simple)
- **GitLab Flow** : main + environment branches
- **Feature Branch** : Une branche par fonctionnalité

**Bonnes pratiques apprises :**
- **Nommage clair** : `feature/auth`, `bugfix/login-error`
- **Branches courtes** : Fusion rapide pour éviter conflits
- **Tests avant fusion** : Validation sur la branche
- **Historique propre** : Commits logiques et messages clairs
- **Nettoyage régulier** : Suppression branches fusionnées

**Commandes essentielles à retenir :**
```bash
git checkout -b feature/nom     # Créer branche
git checkout main              # Changer branche
git merge feature/nom          # Fusionner
git branch -d feature/nom      # Supprimer
git log --oneline --graph      # Visualiser
```

**Cas d'usage dans le monde réel :**
- **Développement équipe** : Chacun sa branche fonctionnalité
- **Release management** : Branches par version
- **Hotfixes** : Corrections urgentes isolées  
- **Expérimentation** : Tests de nouvelles technologies

Vous pouvez désormais organiser vos projets avec des branches et collaborer efficacement en équipe !

---
