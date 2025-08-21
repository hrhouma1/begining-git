
<a name="table-des-matieres"></a>

## Table des matières


### **Objectif :**
Ce guide vous apprendra à synchroniser votre dépôt local Git avec GitHub. Vous apprendrez à pousser vos changements locaux vers GitHub, à récupérer les changements distants, et à garder votre dépôt à jour avec les dernières modifications de votre équipe.



## **Partie 1 : Théorie**

### **Qu'est-ce que la synchronisation Git avec GitHub ?**

La synchronisation entre un dépôt Git local et un dépôt GitHub permet de maintenir les deux à jour. Voici les actions principales :

1. **`git push`** : Envoie vos commits locaux vers le dépôt GitHub.
2. **`git pull`** : Récupère les modifications effectuées dans le dépôt GitHub par d'autres collaborateurs et les fusionne avec votre dépôt local.
3. **`git fetch`** : Récupère les modifications du dépôt GitHub, mais ne les fusionne pas automatiquement. Vous devez ensuite examiner ou fusionner ces modifications manuellement.
4. **`git clone`** : Télécharge un dépôt GitHub dans un nouveau répertoire local.



## **Partie 2 : Pratique**

Nous allons maintenant synchroniser notre projet local avec GitHub.

### **Étape 1 : Créer un projet et le connecter à GitHub**

Nous allons créer un nouveau projet local et apprendre à le synchroniser avec GitHub.

```bash
mkdir projet-sync-github
cd projet-sync-github
git init
```

**Structure du projet :**
```
projet-sync-github/
├── README.md
├── main.py
├── docs.md
└── config.json
```

Créons nos fichiers initiaux :

```bash
# Créer les fichiers du projet
echo "# Projet Synchronisation GitHub
Ce projet démontre la synchronisation entre Git local et GitHub." > README.md

echo "#!/usr/bin/env python3
def main():
    print('Application de démonstration GitHub sync')
    print('Version 1.0')

if __name__ == '__main__':
    main()" > main.py

echo "# Documentation du projet

## Installation
1. Cloner le dépôt
2. Exécuter main.py

## Utilisation
Ce projet sert d'exemple pour la synchronisation GitHub." > docs.md

echo '{
  "name": "projet-sync-github",
  "version": "1.0.0",
  "description": "Démonstration synchronisation GitHub"
}' > config.json

# Premier commit
git add .
git commit -m "Initialisation du projet avec structure de base"
```

**Note :** Pour la suite, vous devrez créer un dépôt GitHub vide sur github.com et obtenir son URL.



### **Étape 2 : Ajouter et committer des modifications locales**

Nous allons maintenant faire quelques modifications dans notre projet local et les pousser vers GitHub.

#### **1. Améliorer le fichier `main.py` :**

Ajoutons une nouvelle fonctionnalité :

```bash
echo "#!/usr/bin/env python3
def main():
    print('Application de démonstration GitHub sync')
    print('Version 1.1 - avec nouvelles fonctionnalités')

def nouvelle_fonction():
    print('Nouvelle fonction ajoutée localement')
    return 'Données traitées'

def afficher_info():
    print('Informations du projet:')
    print('- Synchronisation GitHub')
    print('- Gestion des conflits')
    print('- Push et pull')

if __name__ == '__main__':
    main()
    nouvelle_fonction()
    afficher_info()" > main.py
```

#### **2. Ajouter les modifications à la zone de staging et les committer :**

1. **Ajouter le fichier modifié à la zone de staging** :

   ```bash
   git add main.py
   ```

2. **Créer un commit pour ces modifications** :

   ```bash
   git commit -m "Amélioration main.py: ajout nouvelles fonctions v1.1"
   ```

#### **3. Ajouter documentation supplémentaire :**

```bash
echo "# FAQ - Questions fréquentes

## Comment synchroniser ?
1. git push pour envoyer
2. git pull pour récupérer

## Gestion des conflits
- Toujours pull avant push
- Résoudre manuellement si nécessaire

## Bonnes pratiques
- Commits fréquents
- Messages clairs" > FAQ.md

git add FAQ.md
git commit -m "Ajout FAQ avec documentation synchronisation"
```



### **Étape 3 : Pousser les modifications locales vers GitHub avec `git push`**

Maintenant nous devons connecter notre projet local à GitHub et y pousser nos modifications.

1. **Connecter le dépôt local à GitHub** :

   Supposons que vous avez créé un dépôt vide sur GitHub nommé `mon-projet-sync`. Ajoutons-le comme origine distante :

   ```bash
   git remote add origin https://github.com/VOTRE-USERNAME/mon-projet-sync.git
   ```

2. **Vérifier la connexion** :

   ```bash
   git remote -v
   ```
   
   Vous devriez voir :
   ```
   origin  https://github.com/VOTRE-USERNAME/mon-projet-sync.git (fetch)
   origin  https://github.com/VOTRE-USERNAME/mon-projet-sync.git (push)
   ```

3. **Pousser pour la première fois** :

   ```bash
   git branch -M main
   git push -u origin main
   ```

**Représentation ASCII de la synchronisation :**
```
Local Repository          GitHub Repository
--         
* FAQ doc                 * FAQ doc                ←─┐
* main.py v1.1           * main.py v1.1            │ push
* Initial commit         * Initial commit         ──┘

git push origin main ──────────────────────────────────→
```

4. **Pour les pushs suivants** :

   ```bash
   git push origin main
   ```

   - **`origin`** : Nom du dépôt distant (GitHub)
   - **`main`** : Branche principale du projet



### **Étape 4 : Récupérer les changements depuis GitHub avec `git pull`**

Simulons le cas où un collaborateur (ou vous-même depuis GitHub.com) a fait des modifications dans le dépôt GitHub que vous devez récupérer.

**Simulation d'un changement distant :**
Imaginez qu'un collaborateur a modifié le fichier `config.json` directement sur GitHub :

```json
{
  "name": "projet-sync-github",
  "version": "1.2.0",
  "description": "Démonstration synchronisation GitHub",
  "author": "Équipe collaborative",
  "features": ["sync", "pull", "push", "conflict-resolution"]
}
```

1. **Récupérer les changements depuis GitHub** :

   ```bash
   git pull origin main
   ```

   - **`pull`** fait deux actions :
     1. **`fetch`** : Récupère les changements distants
     2. **`merge`** : Les fusionne avec votre travail local

**Représentation ASCII du pull :**
```
GitHub Repository         Local Repository
        --
* config v1.2.0    ────→  * config v1.2.0      ←─┐
* FAQ doc                 * FAQ doc              │ pull
* main.py v1.1            * main.py v1.1         │
* Initial commit          * Initial commit     ──┘

git pull origin main ←──────────────────────────────────
```

2. **Vérifier les changements** :

   ```bash
   git log --oneline
   git status
   ```

**Cas de conflit potentiel :**
Si vous et un collaborateur modifiez le même fichier différemment, un **conflit de fusion** peut survenir :

```bash
Auto-merging config.json
CONFLICT (content): Merge conflict in config.json
Automatic merge failed; fix conflicts and then commit the result.
```



### **Étape 5 : Travailler avec `git fetch` pour plus de contrôle**

Parfois vous voulez récupérer les changements distants sans les fusionner automatiquement pour les inspecter d'abord :

1. **Récupérer sans fusionner** :

   ```bash
   git fetch origin
   ```

2. **Inspecter les changements** :

   ```bash
   git log HEAD..origin/main    # Voir les nouveaux commits distants
   git diff HEAD origin/main    # Voir les différences
   ```

3. **Décider de fusionner** :

   ```bash
   git merge origin/main        # Fusionner manuellement
   # OU
   git pull origin main        # Equivalent à fetch + merge
   ```

**Différence fetch vs pull :**
```
git fetch:  GitHub ──→ Local (staging) ──→ (manuel) ──→ Working Directory
git pull:   GitHub ─────────────────────────────────→ Working Directory
```



### **Étape 6 : Créer une nouvelle branche et la pousser vers GitHub**

Créons une branche pour travailler sur une nouvelle fonctionnalité sans affecter main.

1. **Créer et basculer sur une nouvelle branche** :

   ```bash
   git checkout -b feature/ameliorations-interface
   ```

2. **Ajouter de nouvelles fonctionnalités** :

   ```bash
   # Créer un fichier interface
   echo "#!/usr/bin/env python3
   class InterfaceUtilisateur:
       def __init__(self):
           self.titre = 'Interface Améliorée v2.0'
           
       def afficher_menu(self):
           print('=== MENU PRINCIPAL ===')
           print('1. Démarrer application')
           print('2. Configuration')
           print('3. Aide')
           
       def traiter_choix(self, choix):
           if choix == '1':
               print('Application démarrée...')
           elif choix == '2':
               print('Configuration ouverte...')
           else:
               print('Option non reconnue')" > interface.py

   git add interface.py
   git commit -m "Ajout interface utilisateur améliorée"
   ```

3. **Pousser la nouvelle branche vers GitHub** :

   ```bash
   git push -u origin feature/ameliorations-interface
   ```

**Représentation ASCII des branches :**
```
GitHub:                     Local:
--                    -
main                        main
├─ Initial commit          ├─ Initial commit  
├─ main.py v1.1            ├─ main.py v1.1
└─ FAQ doc                 └─ FAQ doc
                                │
feature/ameliorations       feature/ameliorations  
└─ interface.py            └─ interface.py
```



### **Étape 7 : Gérer les conflits lors de la synchronisation**

Lorsque vous et un collaborateur modifiez le même fichier, un conflit peut survenir.

**Exemple de conflit dans `config.json` :**

```bash
<<<<<<< HEAD
{
  "version": "1.1.0",
  "description": "Version locale"
}
=======
{
  "version": "1.2.0", 
  "description": "Version GitHub"
}
>>>>>>> origin/main
```

**Résolution :**

1. **Éditer le fichier** - Choisir la bonne version :
   ```json
   {
     "version": "1.2.0",
     "description": "Version fusionnée et résolue"
   }
   ```

2. **Finaliser la résolution** :
   ```bash
   git add config.json
   git commit -m "Résolution conflit config.json - fusion versions"
   git push origin main
   ```

**Bonnes pratiques pour éviter les conflits :**
- Toujours `git pull` avant `git push`
- Travailler sur des fichiers différents si possible
- Communiquer avec l'équipe sur les modifications importantes
- Faire des commits fréquents et petits



## **Résumé des commandes**

### **Commandes complètes du tutoriel**

1. **Créer et initialiser le projet** :
   ```bash
   mkdir projet-sync-github && cd projet-sync-github
   git init
   echo "# Projet Synchronisation GitHub" > README.md
   echo "def main(): print('App v1.0')" > main.py
   echo '{"name": "projet-sync", "version": "1.0.0"}' > config.json
   git add . && git commit -m "Initialisation du projet"
   ```

2. **Connecter à GitHub et pousser** :
   ```bash
   git remote add origin https://github.com/USERNAME/mon-projet-sync.git
   git branch -M main
   git push -u origin main
   ```

3. **Développement et synchronisation** :
   ```bash
   # Faire des modifications
   echo "def nouvelle_fonction(): pass" >> main.py
   git add main.py
   git commit -m "Amélioration fonctionnalités"
   
   # Synchroniser
   git pull origin main      # Récupérer changements distants
   git push origin main      # Envoyer changements locaux
   ```

### **Commandes essentielles synchronisation GitHub**

4. **Gestion des remotes** :
   ```bash
   git remote -v                    # Voir les dépôts distants
   git remote add origin <url>      # Ajouter un dépôt distant
   git remote remove origin         # Supprimer un dépôt distant
   ```

5. **Synchronisation** :
   ```bash
   git push origin main            # Pousser vers GitHub
   git pull origin main            # Récupérer depuis GitHub
   git fetch origin                # Récupérer sans fusionner
   git clone <url>                 # Cloner un dépôt existant
   ```

6. **Gestion des branches** :
   ```bash
   git checkout -b feature/nouvelle     # Créer nouvelle branche
   git push -u origin feature/nouvelle  # Pousser nouvelle branche
   git pull origin feature/nouvelle     # Tirer branche spécifique
   ```

### **Résolution de conflits**
```bash
# Lors d'un conflit
git pull origin main          # Conflit détecté
# -> Éditer fichiers en conflit
git add fichier-resolu.txt    # Marquer comme résolu
git commit -m "Résolution conflit"
git push origin main
```



## **Conclusion**

**Félicitations !** Vous avez maîtrisé la **synchronisation GitHub** avec un projet exemple complet et autonome.

**Ce que vous avez appris :**
- Créer un projet local et le connecter à GitHub
- Utiliser `git push` pour envoyer vos modifications
- Utiliser `git pull` pour récupérer les changements distants  
- Gérer les branches locales et distantes
- Résoudre les conflits de fusion
- Comprendre la différence entre `fetch` et `pull`

**Workflow de synchronisation complet :**
```
1. Développement local:
   git add . → git commit -m "..." 

2. Synchronisation avant push:
   git pull origin main

3. Résolution conflits (si nécessaire):
   Éditer fichiers → git add → git commit

4. Envoi vers GitHub:
   git push origin main
```

**Commandes essentielles à retenir :**
- **`git remote add origin <url>`** : Connecter à GitHub
- **`git push origin main`** : Envoyer modifications
- **`git pull origin main`** : Récupérer modifications
- **`git fetch origin`** : Récupérer sans fusionner
- **`git clone <url>`** : Cloner un dépôt existant

**Bonnes pratiques apprises :**
- **Pull avant push** : Toujours récupérer avant envoyer
- **Commits fréquents** : Petits commits réguliers
- **Messages clairs** : Descriptions précises des changements
- **Branches de feature** : Isoler le développement de nouvelles fonctionnalités
- **Communication équipe** : Coordonner les modifications importantes

**Cas d'usage dans le monde réel :**
- **Projet personnel** : Sauvegarder et accéder depuis plusieurs machines
- **Travail en équipe** : Collaborer sur le même code
- **Open Source** : Contribuer à des projets communautaires
- **Portfolio** : Montrer son travail aux employeurs potentiels

**Étapes suivantes suggérées :**
- Pratiquer avec vos propres projets
- Explorer GitHub Desktop pour une interface graphique
- Apprendre les Pull Requests pour la collaboration
- Découvrir GitHub Actions pour l'automatisation

Vous pouvez désormais collaborer efficacement sur GitHub et maintenir vos projets synchronisés !


