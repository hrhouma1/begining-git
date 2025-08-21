
<a name="table-des-matieres"></a>

## Table des matières


### **Objectif :**
Ce guide vous apprendra à utiliser le fichier `.gitignore` pour exclure certains fichiers et dossiers du suivi par Git. Vous allez comprendre ce qu’est un fichier `.gitignore`, pourquoi il est important, et comment l’utiliser efficacement dans votre projet.



## **Partie 1 : Théorie**

### **Qu'est-ce qu'un fichier `.gitignore` ?**

- **Le fichier `.gitignore`** est un fichier texte spécial qui indique à Git quels fichiers ou répertoires ne doivent **pas** être suivis ou inclus dans les commits. Par exemple, vous pouvez l'utiliser pour exclure des fichiers de configuration, des dossiers de build, des fichiers temporaires, ou des fichiers contenant des informations sensibles.

### **Pourquoi utiliser `.gitignore` ?**

- **Éviter les fichiers inutiles** : Par exemple, les fichiers générés automatiquement comme les fichiers de log ou les dossiers `node_modules` dans un projet Node.js.
- **Protéger les informations sensibles** : Comme les fichiers contenant des mots de passe ou des informations d'accès (fichiers `.env`).
- **Réduire la taille du dépôt** : En excluant les fichiers inutiles, votre dépôt Git reste plus léger et plus rapide.



## **Partie 2 : Pratique**

Nous allons maintenant créer un projet exemple pour apprendre à utiliser le fichier `.gitignore`. Vous allez apprendre à créer et configurer un fichier `.gitignore`, puis à tester son effet sur les fichiers de votre projet.

### **Étape 1 : Créer un projet exemple pour .gitignore**

Nous allons créer un projet simple pour démontrer l'utilisation du fichier `.gitignore`.

```bash
mkdir projet-gitignore-demo
cd projet-gitignore-demo
git init
```

**Structure du projet que nous allons créer :**
```
projet-gitignore-demo/
├── README.md
├── main.py
├── config.json
├── .env                 (fichier sensible à ignorer)
├── build/               (dossier généré à ignorer)
├── logs/                (dossier de logs à ignorer)
└── .gitignore
```

Créons d'abord nos fichiers de base :

```bash
# Créer les fichiers principaux
echo "# Projet de démonstration .gitignore" > README.md
echo "print('Application principale')" > main.py
echo '{"name": "MonApp", "version": "1.0.0"}' > config.json

# Premier commit initial
git add .
git commit -m "Initialisation du projet avec fichiers de base"
```



### **Étape 2 : Créer un fichier `.gitignore`**

Nous allons créer un fichier `.gitignore` pour indiquer à Git quels fichiers et dossiers ne doivent pas être suivis.

1. **Créer le fichier `.gitignore`** :

   Tapez la commande suivante pour créer un fichier `.gitignore` à la racine du projet :

   ```bash
   touch .gitignore
   ```

   Vous venez de créer un fichier texte spécial appelé `.gitignore`.

2. **Ouvrir le fichier `.gitignore`** :

   Ouvrez le fichier `.gitignore` dans votre éditeur de texte (VSCode, Sublime Text, etc.).



### **Étape 3 : Créer et configurer le fichier .gitignore**

Nous allons maintenant créer un fichier `.gitignore` et y ajouter des règles pour ignorer différents types de fichiers et dossiers.

1. **Créer le fichier `.gitignore`** :

   ```bash
   touch .gitignore
   ```

2. **Ajouter des règles d'ignorage dans le fichier** :

   Ouvrez le fichier `.gitignore` et ajoutez le contenu suivant :

   ```bash
   cat > .gitignore << 'EOF'
# Fichiers de configuration sensibles
.env
secrets.txt
config/passwords.conf

# Fichiers de logs
*.log
logs/
debug.txt

# Dossiers de build et cache
build/
dist/
cache/
__pycache__/
*.pyc

# Fichiers temporaires
tmp/
temp/
*.tmp
*.temp

# Fichiers système
.DS_Store
Thumbs.db
*.swp
*.swo

# Dossiers IDE
.vscode/
.idea/
*.sublime-*

# Fichiers de sauvegarde
*.bak
*.backup
*~

# Ignorer tous les fichiers d'un type sauf certains
uploads/*
!uploads/README.md
!uploads/*.jpg
EOF
   ```

**Explication des règles :**
- `*.log` : Ignore tous les fichiers avec extension .log
- `logs/` : Ignore le dossier entier logs/
- `build/` : Ignore le dossier de compilation
- `!uploads/*.jpg` : Exception - garde les fichiers .jpg dans uploads/



### **Étape 4 : Tester le fichier `.gitignore`**

Nous allons maintenant créer des fichiers et dossiers de test pour vérifier que Git ignore bien ceux spécifiés dans `.gitignore`.

#### **1. Créer des fichiers sensibles à ignorer :**

```bash
# Créer un fichier .env avec des secrets
echo "DATABASE_PASSWORD=super_secret_password
API_KEY=abc123xyz789
SECRET_TOKEN=dont_commit_this" > .env

# Créer un fichier de secrets
echo "admin_password=secret123" > secrets.txt
```

#### **2. Créer des dossiers et fichiers de build/cache :**

```bash
# Créer le dossier build avec des fichiers générés
mkdir -p build dist logs cache

# Fichiers dans build/
echo "compiled code here" > build/main.js
echo "minified code" > dist/app.min.js

# Fichiers de logs
echo "ERROR: Something went wrong" > logs/error.log
echo "DEBUG: App started" > debug.txt

# Fichiers cache Python
mkdir -p __pycache__
echo "bytecode" > __pycache__/main.pyc
```

#### **3. Créer des fichiers temporaires et système :**

```bash
# Fichiers temporaires
mkdir tmp
echo "temporary data" > tmp/cache.tmp
echo "temp file" > temp_file.temp

# Fichiers système (simulation)
echo "mac finder info" > .DS_Store
echo "windows thumbnail" > Thumbs.db
```

#### **4. Créer le dossier uploads avec exceptions :**

```bash
# Créer dossier uploads avec différents types de fichiers
mkdir uploads
echo "Image JPG autorisée" > uploads/photo.jpg
echo "Fichier TXT ignoré" > uploads/data.txt
echo "Documentation autorisée" > uploads/README.md
```

#### **5. Vérifier que Git ignore correctement :**

Maintenant testons notre configuration `.gitignore` :

```bash
git status
```

**Résultat attendu :**
- Git devrait SEULEMENT voir le fichier `.gitignore` comme nouveau
- AUCUN des fichiers/dossiers suivants ne devrait apparaître :
  - `.env`, `secrets.txt`
  - `build/`, `dist/`, `logs/`, `cache/`
  - `tmp/`, `__pycache__/`
  - `.DS_Store`, `Thumbs.db`
  - `uploads/data.txt` (mais `uploads/photo.jpg` et `uploads/README.md` devraient être visibles)

#### **6. Visualisation de l'état :**

```
Structure du projet après test:
projet-gitignore-demo/
├── README.md           ✓ (suivi par Git)
├── main.py            ✓ (suivi par Git)
├── config.json        ✓ (suivi par Git)
├── .gitignore         ✓ (nouveau, à ajouter)
├── .env               ✗ (ignoré)
├── secrets.txt        ✗ (ignoré)
├── build/             ✗ (ignoré)
├── logs/              ✗ (ignoré)
├── tmp/               ✗ (ignoré)
└── uploads/
    ├── photo.jpg      ✓ (exception, suivi)
    ├── README.md      ✓ (exception, suivi)
    └── data.txt       ✗ (ignoré)
```



### **Étape 5 : Ajouter et committer le fichier `.gitignore`**

Une fois que vous avez configuré votre fichier `.gitignore`, vous devez l'ajouter à la zone de staging et le committer pour le partager avec votre équipe.

1. **Ajouter le fichier `.gitignore` à la zone de staging** :

   ```bash
   git add .gitignore
   ```

2. **Créer un commit pour le fichier `.gitignore`** :

   ```bash
   git commit -m "Ajout du fichier .gitignore pour exclure les fichiers sensibles et temporaires"
   ```



### **Étape 6 : Finaliser le projet**

Maintenant que votre `.gitignore` fonctionne correctement, finalisez votre projet :

1. **Ajouter les fichiers autorisés** :

   ```bash
   # Ajouter les fichiers exception uploads
   git add uploads/photo.jpg uploads/README.md
   ```

2. **Vérifier une dernière fois** :

   ```bash
   git status
   ```
   
   Vous devriez voir seulement :
   - `.gitignore` (modifié)
   - `uploads/photo.jpg` (nouveau)  
   - `uploads/README.md` (nouveau)

3. **Committer le tout** :

   ```bash
   git add .gitignore
   git commit -m "Ajout fichier .gitignore et fichiers autorisés"
   ```

**Résultat final :**
```
Commits dans le projet:
* b4e7f3a Ajout fichier .gitignore et fichiers autorisés
* 2a8c9e1 Initialisation du projet avec fichiers de base

Fichiers suivis par Git:
✓ README.md
✓ main.py  
✓ config.json
✓ .gitignore
✓ uploads/photo.jpg
✓ uploads/README.md

Fichiers/dossiers ignorés (non suivis):
✗ .env
✗ secrets.txt
✗ build/, dist/, logs/, cache/
✗ tmp/, __pycache__/
✗ .DS_Store, Thumbs.db
✗ uploads/data.txt
```

*Optionnel : Si vous travaillez avec un dépôt distant :*
```bash
git push origin main
```



## **Résumé des commandes**

### **Commandes complètes du tutoriel**

1. **Créer le projet exemple** :
   ```bash
   mkdir projet-gitignore-demo && cd projet-gitignore-demo
   git init
   echo "# Projet de démonstration .gitignore" > README.md
   echo "print('Application principale')" > main.py
   echo '{"name": "MonApp", "version": "1.0.0"}' > config.json
   git add . && git commit -m "Initialisation du projet avec fichiers de base"
   ```

2. **Créer et configurer le fichier `.gitignore`** :
   ```bash
   cat > .gitignore << 'EOF'
   # Fichiers sensibles
   .env
   secrets.txt
   
   # Logs et cache
   *.log
   logs/
   build/
   __pycache__/
   
   # Fichiers temporaires
   tmp/
   *.tmp
   
   # Fichiers système
   .DS_Store
   Thumbs.db
   
   # Exceptions
   uploads/*
   !uploads/*.jpg
   !uploads/README.md
   EOF
   ```

3. **Créer des fichiers de test** :
   ```bash
   # Fichiers sensibles (seront ignorés)
   echo "DATABASE_PASSWORD=secret" > .env
   echo "admin_password=secret123" > secrets.txt
   
   # Dossiers et fichiers divers
   mkdir -p build logs tmp uploads __pycache__
   echo "compiled" > build/main.js
   echo "ERROR: test" > logs/error.log
   echo "temp data" > tmp/cache.tmp
   
   # Fichiers uploads avec exceptions
   echo "Image autorisée" > uploads/photo.jpg
   echo "Doc autorisée" > uploads/README.md
   echo "Fichier ignoré" > uploads/data.txt
   ```

4. **Tester et finaliser** :
   ```bash
   git status                                    # Vérifier ce qui est ignoré
   git add uploads/photo.jpg uploads/README.md   # Ajouter exceptions
   git add .gitignore                           # Ajouter le .gitignore
   git commit -m "Ajout fichier .gitignore et fichiers autorisés"
   ```

### **Patterns .gitignore les plus courants**
```bash
# Fichiers sensibles
.env
*.key
secrets/

# Build et cache  
build/
dist/
node_modules/
__pycache__/
*.pyc

# Logs et debug
*.log
debug.txt

# Temporaires
tmp/
*.tmp
*.temp

# IDE et OS
.vscode/
.idea/
.DS_Store
Thumbs.db

# Exceptions
uploads/*
!uploads/*.jpg
!uploads/README.md
```



## **Conclusion**

**Félicitations !** Vous avez maîtrisé le fichier **.gitignore** avec un projet exemple complet et autonome.

**Ce que vous avez appris :**
- Créer un projet Git avec différents types de fichiers
- Configurer un fichier `.gitignore` complet et professionnel
- Utiliser des patterns avancés (wildcards, exceptions, dossiers)
- Tester efficacement les règles d'ignorage
- Gérer les fichiers sensibles et temporaires

**Avantages du fichier .gitignore :**
- **Sécurité** : Protection des informations sensibles (mots de passe, clés API)
- **Performance** : Dépôts plus légers (pas de fichiers build/cache)
- **Propreté** : Historique Git sans fichiers inutiles
- **Collaboration** : Évite les conflits sur les fichiers générés

**Patterns essentiels à retenir :**
- `*.ext` : Ignore toute extension spécifique
- `dossier/` : Ignore un dossier entier
- `!exception` : Crée une exception à une règle
- `**/*.temp` : Ignore récursivement dans tous les sous-dossiers

**Bonnes pratiques :**
- Créer le `.gitignore` dès le début du projet
- Utiliser des templates spécifiques au langage/framework
- Tester les règles avant de committer
- Documenter les exceptions importantes
- Partager le `.gitignore` avec l'équipe

**Types de fichiers couramment ignorés :**
- Fichiers de configuration avec secrets (`.env`, `config.local`)
- Dossiers de build (`build/`, `dist/`, `target/`)
- Cache des langages (`__pycache__/`, `node_modules/`)
- Fichiers temporaires (`*.tmp`, `*.temp`, `*.log`)
- Fichiers IDE/OS (`.vscode/`, `.DS_Store`)

Vous pouvez désormais créer des projets Git propres et sécurisés grâce au fichier `.gitignore` !


