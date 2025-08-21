
<a name="table-des-matieres"></a>

## Table des matières


### **Objectif :**
Ce guide vous apprendra à utiliser la commande **`git cherry-pick`**, qui permet de sélectionner des commits spécifiques d'une branche pour les appliquer à une autre. Vous apprendrez quand et comment utiliser cette commande dans un projet Git.



## **Partie 1 : Théorie**

### **Qu'est-ce que `git cherry-pick` ?**

- **`git cherry-pick`** est une commande Git qui permet d'appliquer un ou plusieurs commits spécifiques d'une branche à une autre sans fusionner toute la branche. Cela peut être utile lorsque vous souhaitez intégrer des modifications précises sans fusionner l'intégralité de l'historique.

### **Quand utiliser `git cherry-pick` ?**

- **Sélection de modifications spécifiques** : Si une branche contient des commits importants que vous voulez appliquer à une autre branche, mais que vous ne souhaitez pas fusionner tout le contenu de la branche, `git cherry-pick` vous permet de le faire.
- **Corrections de bugs** : Lorsque vous avez corrigé un bug dans une branche et que vous voulez appliquer cette correction à d'autres branches sans fusionner toutes les autres modifications.
  
### **Comment fonctionne `git cherry-pick` ?**

- `git cherry-pick` applique un ou plusieurs commits d'une branche source à une autre branche. Chaque commit spécifié par son **ID** sera appliqué à la branche actuelle sans toucher aux autres commits de la branche source.



## **Partie 2 : Pratique**

Nous allons maintenant utiliser **`git cherry-pick`** avec un **projet générique** pour appliquer des commits spécifiques d'une branche à une autre.

### **Étape 1 : Créer un projet pour démontrer Cherry Pick**

Créons un projet d'application de gestion de tâches pour démontrer `git cherry-pick` :

```bash
mkdir projet-cherry-pick-demo
cd projet-cherry-pick-demo
git init
```

**Structure du projet :**
```
projet-cherry-pick-demo/
├── README.md
├── src/
│   ├── main.py
│   ├── utils.js
│   └── styles.css
└── config.json
```

**Initialisation du projet :**

```bash
# Créer la structure
mkdir src

echo "# Projet Cherry Pick Demo
Application de gestion de tâches pour démontrer git cherry-pick.

## Fonctionnalités
- Gestion des tâches en Python
- Interface utilisateur en JavaScript
- Configuration flexible" > README.md

echo "# Application de gestion de tâches
import json
from datetime import datetime

class TaskManager:
    def __init__(self):
        self.tasks = []
    
    def add_task(self, title, priority='normal'):
        task = {
            'id': len(self.tasks) + 1,
            'title': title,
            'priority': priority,
            'created_at': datetime.now().isoformat(),
            'completed': False
        }
        self.tasks.append(task)
        return task

if __name__ == '__main__':
    manager = TaskManager()
    print('Gestionnaire de tâches initialisé')" > src/main.py

echo "// Utilitaires JavaScript pour l'interface
console.log('Utilitaires chargés');

function formatDate(dateString) {
    const date = new Date(dateString);
    return date.toLocaleDateString('fr-FR');
}

function validateTask(title) {
    return title && title.trim().length > 0;
}" > src/utils.js

echo "/* Styles pour l'application de tâches */
.task-container {
    max-width: 600px;
    margin: 0 auto;
    padding: 20px;
    font-family: Arial, sans-serif;
}

.task-item {
    padding: 10px;
    margin: 5px 0;
    border: 1px solid #ddd;
    border-radius: 5px;
    background-color: #f9f9f9;
}" > src/styles.css

echo '{
  "name": "cherry-pick-demo",
  "version": "1.0.0",
  "description": "Démonstration git cherry-pick",
  "main": "src/main.py",
  "dependencies": {
    "python": ">=3.8"
  }
}' > config.json

# Premier commit
git add .
git commit -m "Version initiale de l'application de tâches"
```



### **Étape 2 : Créer deux branches avec des fonctionnalités différentes**

Nous allons créer deux branches, `feature/priorites` et `feature/notifications`, et faire des commits spécifiques que nous appliquerons avec `git cherry-pick`.

1. **Créer la branche `feature/priorites`** :

   ```bash
   git checkout -b feature/priorites
   ```

2. **Ajouter la gestion des priorités dans `main.py`** :

   ```bash
   # Ajouter des méthodes de priorité
   echo "
    def set_priority(self, task_id, priority):
        for task in self.tasks:
            if task['id'] == task_id:
                task['priority'] = priority
                return task
        return None
    
    def get_high_priority_tasks(self):
        return [task for task in self.tasks if task['priority'] == 'high']" >> src/main.py
   ```

3. **Committer cette modification** :

   ```bash
   git add src/main.py
   git commit -m "Ajout gestion des priorités des tâches"
   ```

4. **Ajouter des styles pour les priorités** :

   ```bash
   echo "
.task-high-priority {
    border-left: 4px solid #e74c3c;
    background-color: #fdf2f2;
}

.task-normal-priority {
    border-left: 4px solid #3498db;
}

.priority-badge {
    display: inline-block;
    padding: 2px 8px;
    border-radius: 12px;
    font-size: 12px;
    font-weight: bold;
}" >> src/styles.css
   ```

5. **Committer les styles des priorités** :

   ```bash
   git add src/styles.css
   git commit -m "Ajout styles pour les priorités des tâches"
   ```

6. **Améliorer les utilitaires JavaScript** :

   ```bash
   echo "
function getPriorityClass(priority) {
    switch(priority) {
        case 'high': return 'task-high-priority';
        case 'normal': return 'task-normal-priority';
        default: return 'task-normal-priority';
    }
}

function sortTasksByPriority(tasks) {
    const priorityOrder = { 'high': 3, 'normal': 2, 'low': 1 };
    return tasks.sort((a, b) => priorityOrder[b.priority] - priorityOrder[a.priority]);
}" >> src/utils.js
   ```

7. **Committer les utilitaires de priorité** :

   ```bash
   git add src/utils.js
   git commit -m "Ajout utilitaires JavaScript pour les priorités"
   ```

8. **Créer une nouvelle branche `feature/notifications`** :

   ```bash
   git checkout main
   git checkout -b feature/notifications
   ```



### **Étape 3 : Utiliser `git cherry-pick` pour appliquer des commits spécifiques**

Nous allons maintenant appliquer des commits spécifiques de `feature/priorites` à `feature/notifications` en utilisant `git cherry-pick`.

**Scénario :** Nous voulons récupérer seulement les **utilitaires JavaScript** de la branche priorités pour la branche notifications, sans prendre les autres modifications.

1. **Voir l'historique de la branche `feature/priorites`** :

   Basculez sur la branche priorités pour voir les commits disponibles :

   ```bash
   git checkout feature/priorites
   git log --oneline
   ```

   Vous devriez voir quelque chose comme ceci :

   ```
   f7e3d2a Ajout utilitaires JavaScript pour les priorités
   b4c8f1e Ajout styles pour les priorités des tâches  
   a9d5c3f Ajout gestion des priorités des tâches
   e1f6b8d Version initiale de l'application de tâches
   ```

2. **Choisir le commit à appliquer** :

   Nous voulons seulement les **utilitaires JavaScript** (commit `f7e3d2a`), pas les styles ni la logique Python.

3. **Basculer sur la branche cible et appliquer le commit** :

   ```bash
   git checkout feature/notifications
   git cherry-pick f7e3d2a
   ```

   **Résultat :** Le commit "Ajout utilitaires JavaScript pour les priorités" est maintenant appliqué à `feature/notifications`.

4. **Vérifier le résultat** :

   ```bash
   git status
   git log --oneline
   ```

   Vous devriez voir :
   ```
   f7e3d2a Ajout utilitaires JavaScript pour les priorités
   e1f6b8d Version initiale de l'application de tâches
   ```

   Et vérifier que le fichier `src/utils.js` contient maintenant les fonctions de priorité.

5. **Cherry-pick multiple - Appliquer seulement certains commits** :

   Si vous voulez récupérer les utilitaires ET les styles (mais pas la logique Python) :

   ```bash
   git cherry-pick b4c8f1e f7e3d2a
   ```

   Ou avec une plage (attention, cela inclut TOUS les commits de la plage) :

   ```bash
   git cherry-pick a9d5c3f..f7e3d2a  # Inclut TOUT entre ces commits
   ```

**Visualisation ASCII avant/après cherry-pick :**

```
AVANT cherry-pick:

feature/priorites                    feature/notifications  
       ↓                                     ↓
       * f7e3d2a (JS utils)                  * e1f6b8d (initial)
       * b4c8f1e (CSS styles)               
       * a9d5c3f (Python logic)             
       * e1f6b8d (initial)                  

APRÈS cherry-pick f7e3d2a:

feature/priorites                    feature/notifications (HEAD)
       ↓                                     ↓  
       * f7e3d2a (JS utils)                  * f7e3d2a (JS utils) ← COPIÉ
       * b4c8f1e (CSS styles)               * e1f6b8d (initial)
       * a9d5c3f (Python logic)             
       * e1f6b8d (initial)                  
```



### **Étape 4 : Gérer les conflits lors du `cherry-pick`**

Parfois, des conflits peuvent survenir lors d'un cherry-pick. Simulons et résolvons un conflit.

**Créer un conflit intentionnel :**

1. **Modifier le même fichier dans `feature/notifications`** :

   ```bash
   git checkout feature/notifications
   
   # Modifier les utilitaires avec une fonction différente
   echo "
   function formatPriority(priority) {
       // Version notifications - approche différente
       return priority.toUpperCase() + '_NOTIFICATION';
   }" >> src/utils.js
   
   git add src/utils.js
   git commit -m "Ajout utilitaires pour notifications"
   ```

2. **Maintenant, essayer un cherry-pick qui va créer un conflit** :

   ```bash
   # Essayer d'appliquer un autre commit qui modifie le même fichier
   git checkout feature/priorites
   
   # Ajouter une autre fonction aux utilitaires
   echo "
   function formatPriority(priority) {
       // Version priorités - approche différente  
       return `[${priority.toUpperCase()}]`;
   }" >> src/utils.js
   
   git add src/utils.js
   git commit -m "Ajout formatage personnalisé des priorités"
   
   # Récupérer l'ID de ce commit
   git log --oneline -1  # Par exemple: g8h4j5k
   ```

3. **Retour sur notifications et cherry-pick qui va créer un conflit** :

   ```bash
   git checkout feature/notifications
   git cherry-pick g8h4j5k  # Remplacer par l'ID réel
   ```

4. **Résoudre le conflit** :

   Git va indiquer un conflit dans `src/utils.js` :

   ```bash
   Auto-merging src/utils.js
   CONFLICT (content): Merge conflict in src/utils.js
   error: could not apply g8h4j5k... Ajout formatage personnalisé des priorités
   ```

   Le fichier ressemblera à :
   ```javascript
   <<<<<<< HEAD
   function formatPriority(priority) {
       // Version notifications - approche différente
       return priority.toUpperCase() + '_NOTIFICATION';
   }
   =======
   function formatPriority(priority) {
       // Version priorités - approche différente  
       return `[${priority.toUpperCase()}]`;
   }
   >>>>>>> g8h4j5k (Ajout formatage personnalisé des priorités)
   ```

5. **Éditer le fichier pour résoudre le conflit** :

   ```javascript
   // Garder les deux approches ou en choisir une
   function formatPriority(priority, type = 'default') {
       switch(type) {
           case 'notification':
               return priority.toUpperCase() + '_NOTIFICATION';
           case 'priority':
               return `[${priority.toUpperCase()}]`;
           default:
               return priority.toUpperCase();
       }
   }
   ```

6. **Finaliser la résolution** :

   ```bash
   git add src/utils.js
   git cherry-pick --continue
   ```

7. **En cas d'abandon** :

   Si le conflit est trop complexe, vous pouvez annuler :

   ```bash
   git cherry-pick --abort  # Annule tout et revient à l'état précédent
   ```



### **Étape 5 : Intégrer et finaliser**

Après avoir appliqué les commits avec `git cherry-pick`, finalisez le workflow :

1. **Pousser les branches vers GitHub (optionnel)** :

   ```bash
   # Pousser la branche avec les modifications cherry-pickées
   git push origin feature/notifications
   
   # Pousser aussi la branche source si nécessaire
   git push origin feature/priorites
   ```

2. **Intégrer dans main (workflow complet)** :

   ```bash
   git checkout main
   git pull origin main  # S'assurer d'avoir la dernière version
   
   # Fusionner la branche avec les cherry-picks
   git merge feature/notifications
   git push origin main
   
   # Nettoyer les branches de développement
   git branch -d feature/notifications
   git branch -d feature/priorites
   ```



### **Étape 6 : Résumé des commandes `git cherry-pick`**

**Commandes essentielles :**

1. **Visualiser l'historique pour choisir les commits** :
   ```bash
   git log --oneline --graph        # Vue graphique
   git log --oneline -n 5           # 5 derniers commits
   git log --oneline feature/source # Commits d'une branche
   ```

2. **Cherry-pick basique** :
   ```bash
   git cherry-pick <commit_id>      # Un seul commit
   git cherry-pick commit1 commit2  # Commits multiples
   git cherry-pick commit1..commit3 # Plage de commits
   ```

3. **Options avancées** :
   ```bash
   git cherry-pick -x <commit_id>   # Ajoute référence au commit original
   git cherry-pick -n <commit_id>   # Applique sans committer
   git cherry-pick --no-commit <id> # Même chose que -n
   ```

4. **Gestion des conflits** :
   ```bash
   git cherry-pick <commit_id>      # Conflit détecté
   # ... résoudre les conflits ...
   git add <fichiers_résolus>
   git cherry-pick --continue       # Finaliser
   
   # OU
   git cherry-pick --abort          # Annuler complètement
   git cherry-pick --quit           # Arrêter mais garder les changements
   ```

5. **Vérifications utiles** :
   ```bash
   git status                       # État pendant cherry-pick
   git diff HEAD~1                  # Voir les changements appliqués
   git log --oneline -5             # Vérifier l'historique
   ```

**Cas d'usage typiques :**

| Situation | Commande | Description |
|--|-|-|
| Bug fix urgent | `git cherry-pick <fix_commit>` | Appliquer un correctif sur plusieurs branches |
| Feature partielle | `git cherry-pick commit1 commit3` | Récupérer seulement certaines parties |
| Backport | `git cherry-pick -x <commit>` | Porter une fonctionnalité vers une version antérieure |
| Test avant merge | `git cherry-pick -n <commit>` | Tester un commit sans l'appliquer définitivement |



### **Conclusion**

**Félicitations !** Vous maîtrisez maintenant **`git cherry-pick`**, une commande puissante pour la gestion précise des commits.

**Ce que vous avez appris :**
- Appliquer des commits spécifiques entre branches
- Gérer les conflits lors du cherry-pick  
- Utiliser les options avancées (`-x`, `-n`, `--continue`, `--abort`)
- Intégrer cherry-pick dans un workflow complet
- Résoudre des situations réelles (corrections urgentes, features partielles)

**Avantages du cherry-pick :**
- **Précision** : Sélectionner exactement les modifications nécessaires
- **Flexibilité** : Éviter les merges complets non désirés
- **Maintenance** : Appliquer des correctifs sur plusieurs branches
- **Contrôle** : Garder un historique propre et intentionnel

**Règles d'or :**
- **Analysez l'historique** avant de cherry-pick
- **Testez** les commits appliqués
- **Documentez** avec `-x` pour traçabilité
- **Nettoyez** les branches après intégration

**Quand utiliser cherry-pick :**
- ✅ Corrections de bugs à appliquer sur plusieurs branches
- ✅ Récupération de fonctionnalités spécifiques
- ✅ Backports vers versions de maintenance
- ❌ Éviter pour des merges complets (utiliser `git merge`)

Cherry-pick vous donne un **contrôle chirurgical** sur votre historique Git. Utilisez cette puissance avec sagesse pour maintenir un projet propre et bien organisé !
