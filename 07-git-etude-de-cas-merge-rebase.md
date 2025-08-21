---
title: "Chapitre 7 - Etude de cas : Merge vs Rebase"
description: "Comprendre les différences entre git merge et git rebase avec un projet exemple complet"
---

---
<a name="table-des-matieres"></a>

## Table des matières
---

1. [Introduction](#introduction)
2. [Partie 1 : Théorie](#partie-1-theorie)
   1. [Git Merge : Qu'est-ce que c'est ?](#git-merge)
   2. [Git Rebase : Qu'est-ce que c'est ?](#git-rebase)  
   3. [Comparaison Merge vs Rebase](#comparaison)
3. [Partie 2 : Pratique](#partie-2-pratique)
   1. [Étape 1 : Créer un projet exemple](#etape-1)
   2. [Étape 2 : Développement parallèle](#etape-2)
   3. [Étape 3 : Merge Fast-forward](#etape-3)
   4. [Étape 4 : Merge Three-way](#etape-4)
   5. [Étape 5 : Gestion des conflits](#etape-5)
   6. [Étape 6 : Démonstration Rebase](#etape-6)
4. [Résumé des commandes](#resume-commandes)
5. [Conclusion](#conclusion)

<br/>
---

<a name="introduction"></a>
## Introduction
---

Cette étude de cas vous permettra de comprendre concrètement les différences entre **Git Merge** et **Git Rebase** à travers un projet exemple complet. Vous apprendrez quand utiliser chaque approche et comment gérer les conflits.

[⬆️ retour à la table des matières](#table-des-matieres)
<br/>

---

<a name="partie-1-theorie"></a>
## 1 - Partie 1 : Théorie
---

<a name="git-merge"></a>
### **Git Merge : Qu'est-ce que c'est ?**

**Git Merge** fusionne deux branches en créant un nouveau commit qui combine l'historique des deux branches.

**Types de merge :**
- **Fast-forward** : Quand la branche à fusionner est directement devant
- **Three-way** : Quand les branches ont divergé, crée un commit de fusion
- **Conflits** : Quand les mêmes lignes sont modifiées différemment

**Avantages :**
- Préserve l'historique complet et chronologique
- Montre clairement où les branches ont divergé
- Plus sûr car ne modifie pas l'historique existant
- Recommandé pour les branches partagées

[⬆️ retour à la table des matières](#table-des-matieres)
<br/>

<a name="git-rebase"></a>
### **Git Rebase : Qu'est-ce que c'est ?**

**Git Rebase** réécrit l'historique en réappliquant les commits d'une branche sur une autre, créant un historique linéaire.

**Processus :**
1. Prend tous les commits de la branche de travail
2. Les "rejoue" un par un sur la branche cible
3. Crée un historique parfaitement linéaire

**Avantages :**
- Historique plus propre et linéaire
- Évite les commits de fusion superflus
- Chronologie claire des développements

**⚠️ Règle d'Or :** Ne jamais rebaser des branches publiques partagées !

[⬆️ retour à la table des matières](#table-des-matieres)
<br/>

<a name="comparaison"></a>
### **Comparaison Merge vs Rebase**

| Critère | Git Merge | Git Rebase |
|---------|-----------|------------|
| **Historique** | Préserve l'historique réel | Réécrit l'historique |
| **Lisibilité** | Montre les divergences | Historique linéaire |
| **Sécurité** | Plus sûr | Risqué sur branches partagées |
| **Utilisation** | Branches publiques | Branches locales |

**Recommandations :**
- **Merge** : Pour intégrer des branches publiques ou partagées
- **Rebase** : Pour nettoyer l'historique de vos branches locales
- **Fast-forward** : Évite les commits de fusion inutiles

[⬆️ retour à la table des matières](#table-des-matieres)
<br/>

---

<a name="partie-2-pratique"></a>
## 2 - Partie 2 : Pratique
---

<a name="etape-1"></a>
### **Étape 1 : Créer un projet exemple pour Merge vs Rebase**

Créons un projet web simple pour démontrer les concepts :

```bash
mkdir projet-merge-rebase-demo
cd projet-merge-rebase-demo
git init
```

**Structure du projet :**
```
projet-merge-rebase-demo/
├── README.md
├── index.html
├── style.css
├── app.js
└── config.json
```

**Initialisation du projet :**

```bash
# Créer les fichiers de base
echo "# Projet Démonstration Merge vs Rebase
Site web simple pour comprendre Git merge et rebase.

## Fonctionnalités
- Page d'accueil
- Styles CSS
- JavaScript interactif" > README.md

echo "<!DOCTYPE html>
<html lang='fr'>
<head>
    <meta charset='UTF-8'>
    <title>Démo Merge vs Rebase</title>
    <link rel='stylesheet' href='style.css'>
</head>
<body>
    <header>
        <h1>Projet de démonstration Git</h1>
    </header>
    <main>
        <p>Version initiale du site</p>
    </main>
    <script src='app.js'></script>
</body>
</html>" > index.html

echo "body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 20px;
    background-color: #f0f0f0;
}

header {
    text-align: center;
    margin-bottom: 20px;
}" > style.css

echo "// Script principal
console.log('Site démarré');

document.addEventListener('DOMContentLoaded', function() {
    console.log('DOM chargé');
});" > app.js

echo '{
  "name": "merge-rebase-demo",
  "version": "1.0.0",
  "description": "Démonstration merge vs rebase"
}' > config.json

# Premier commit
git add .
git commit -m "Version initiale du site web"
```

[⬆️ retour à la table des matières](#table-des-matieres)
<br/>

<a name="etape-2"></a>
### **Étape 2 : Développement parallèle avec branches**

Créons deux branches pour simuler un développement parallèle :

```bash
# Créer branche pour fonctionnalité header
git checkout -b feature/header-ameliore

# Améliorer le header
echo "<!DOCTYPE html>
<html lang='fr'>
<head>
    <meta charset='UTF-8'>
    <title>Démo Merge vs Rebase</title>
    <link rel='stylesheet' href='style.css'>
</head>
<body>
    <header>
        <h1>Projet de démonstration Git</h1>
        <nav>
            <ul>
                <li><a href='#'>Accueil</a></li>
                <li><a href='#'>À propos</a></li>
            </ul>
        </nav>
    </header>
    <main>
        <p>Version initiale du site</p>
    </main>
    <script src='app.js'></script>
</body>
</html>" > index.html

git add index.html
git commit -m "Ajout navigation dans header"
```

**Visualisation de l'état actuel :**
```
main
  ↓
  * Version initiale du site web
  │
  └── feature/header-ameliore (HEAD)
      * Ajout navigation dans header
```

[⬆️ retour à la table des matières](#table-des-matieres)
<br/>

<a name="etape-3"></a>
### **Étape 3 : Merge Fast-forward**

Le merge fast-forward se produit quand la branche à fusionner est directement devant :

```bash
# Retour sur main pour fusion
git checkout main

# Merge fast-forward
git merge feature/header-ameliore
```

**Résultat :**
```bash
git log --oneline --graph
```

**Visualisation après merge fast-forward :**
```
main (HEAD)
  ↓
  * Ajout navigation dans header
  ↓
  * Version initiale du site web
```

**Caractéristiques du fast-forward :**
- Aucun commit de fusion créé
- Le pointeur main avance simplement
- Historique linéaire préservé

[⬆️ retour à la table des matières](#table-des-matieres)
<br/>

<a name="etape-4"></a>
### **Étape 4 : Merge Three-way avec commit de fusion**

Créons une situation où les branches divergent :

```bash
# Créer une nouvelle branche depuis main
git checkout -b feature/styles-avances

# Améliorer les styles
echo "body {
    font-family: 'Segoe UI', Arial, sans-serif;
    margin: 0;
    padding: 20px;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
}

header {
    text-align: center;
    margin-bottom: 20px;
    background: rgba(255,255,255,0.1);
    padding: 20px;
    border-radius: 10px;
}

nav ul {
    list-style: none;
    padding: 0;
    display: flex;
    justify-content: center;
    gap: 20px;
}

nav a {
    color: white;
    text-decoration: none;
    font-weight: bold;
}" > style.css

git add style.css
git commit -m "Ajout styles avancés avec dégradé"

# Parallèlement, modifier main
git checkout main

# Modifier le contenu principal
echo "<!DOCTYPE html>
<html lang='fr'>
<head>
    <meta charset='UTF-8'>
    <title>Démo Merge vs Rebase - Site Professionnel</title>
    <link rel='stylesheet' href='style.css'>
</head>
<body>
    <header>
        <h1>Projet de démonstration Git</h1>
        <nav>
            <ul>
                <li><a href='#'>Accueil</a></li>
                <li><a href='#'>À propos</a></li>
            </ul>
        </nav>
    </header>
    <main>
        <h2>Bienvenue sur notre site</h2>
        <p>Ce site démontre l'utilisation de Git merge et rebase.</p>
        <p>Contenu ajouté sur la branche main.</p>
    </main>
    <script src='app.js'></script>
</body>
</html>" > index.html

git add index.html
git commit -m "Amélioration contenu principal et titre"

# Maintenant merge three-way
git merge feature/styles-avances
```

**Visualisation après merge three-way :**
```
main (HEAD)
  ↓
  *   Merge branch 'feature/styles-avances' [COMMIT DE FUSION]
  |\
  | * Ajout styles avancés avec dégradé
  |/
  * Amélioration contenu principal et titre
  ↓
  * Ajout navigation dans header
  ↓
  * Version initiale du site web
```

[⬆️ retour à la table des matières](#table-des-matieres)
<br/>

<a name="etape-5"></a>
### **Étape 5 : Gestion des conflits de fusion**

Créons intentionnellement un conflit pour apprendre à le résoudre :

```bash
# Créer nouvelle branche
git checkout -b feature/footer

# Ajouter footer dans index.html
echo "<!DOCTYPE html>
<html lang='fr'>
<head>
    <meta charset='UTF-8'>
    <title>Démo Merge vs Rebase - Site Professionnel</title>
    <link rel='stylesheet' href='style.css'>
</head>
<body>
    <header>
        <h1>Projet de démonstration Git</h1>
        <nav>
            <ul>
                <li><a href='#'>Accueil</a></li>
                <li><a href='#'>À propos</a></li>
            </ul>
        </nav>
    </header>
    <main>
        <h2>Bienvenue sur notre site</h2>
        <p>Ce site démontre l'utilisation de Git merge et rebase.</p>
        <p>Version avec footer ajouté sur branche feature.</p>
    </main>
    <footer>
        <p>&copy; 2024 - Démonstration Git Merge vs Rebase</p>
    </footer>
    <script src='app.js'></script>
</body>
</html>" > index.html

git add index.html
git commit -m "Ajout footer et modification contenu"

# Retour sur main et modification conflictuelle
git checkout main

echo "<!DOCTYPE html>
<html lang='fr'>
<head>
    <meta charset='UTF-8'>
    <title>Démo Merge vs Rebase - Site Professionnel</title>
    <link rel='stylesheet' href='style.css'>
</head>
<body>
    <header>
        <h1>Projet de démonstration Git</h1>
        <nav>
            <ul>
                <li><a href='#'>Accueil</a></li>
                <li><a href='#'>À propos</a></li>
            </ul>
        </nav>
    </header>
    <main>
        <h2>Bienvenue sur notre site</h2>
        <p>Ce site démontre l'utilisation de Git merge et rebase.</p>
        <p>Version modifiée sur main avec contenu différent.</p>
        <p>Information supplémentaire ajoutée sur main.</p>
    </main>
    <script src='app.js'></script>
</body>
</html>" > index.html

git add index.html
git commit -m "Modification contenu sur main"

# Tentative de merge -> CONFLIT !
git merge feature/footer
```

**Résultat du conflit :**
```bash
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

**Résolution du conflit dans index.html :**

Le fichier contiendra :
```html
<<<<<<< HEAD
        <p>Version modifiée sur main avec contenu différent.</p>
        <p>Information supplémentaire ajoutée sur main.</p>
    </main>
    <script src='app.js'></script>
=======
        <p>Version avec footer ajouté sur branche feature.</p>
    </main>
    <footer>
        <p>&copy; 2024 - Démonstration Git Merge vs Rebase</p>
    </footer>
    <script src='app.js'></script>
>>>>>>> feature/footer
```

**Éditer pour résoudre :**
```html
        <p>Site démontre merge et rebase - version fusionnée.</p>
        <p>Information supplémentaire ajoutée sur main.</p>
    </main>
    <footer>
        <p>&copy; 2024 - Démonstration Git Merge vs Rebase</p>
    </footer>
    <script src='app.js'></script>
```

```bash
# Finaliser la résolution
git add index.html
git commit -m "Résolution conflit: fusion contenu main + footer"
```

**Explication des marqueurs de conflit :**
- `<<<<<<< HEAD` : Début du conflit, contenu de la branche courante
- `=======` : Séparateur entre les deux versions
- `>>>>>>> feature/footer` : Fin du conflit, contenu de la branche à fusionner

[⬆️ retour à la table des matières](#table-des-matieres)
<br/>

<a name="etape-6"></a>
### **Étape 6 : Démonstration pratique de Rebase**

Créons une situation pour démontrer rebase :

```bash
# Nouvelle branche pour JavaScript
git checkout -b feature/javascript-interactif

# Améliorer le JavaScript
echo "// Script principal amélioré
console.log('Site démarré avec fonctionnalités avancées');

document.addEventListener('DOMContentLoaded', function() {
    console.log('DOM chargé');
    
    // Ajouter interactivité
    const header = document.querySelector('h1');
    if (header) {
        header.addEventListener('click', function() {
            header.style.color = header.style.color === 'gold' ? '' : 'gold';
        });
    }
    
    // Animation au scroll
    window.addEventListener('scroll', function() {
        const scrolled = window.pageYOffset;
        const rate = scrolled * -0.5;
        document.body.style.transform = 'translate3d(0, ' + rate + 'px, 0)';
    });
});

// Fonction utilitaire
function toggleTheme() {
    document.body.classList.toggle('dark-theme');
}" > app.js

git add app.js
git commit -m "JavaScript interactif: click header + scroll effects"

# Simuler développement parallèle sur main
git checkout main

# Mise à jour de configuration
echo '{
  "name": "merge-rebase-demo",
  "version": "2.0.0",
  "description": "Démonstration merge vs rebase - Version avancée",
  "features": ["responsive", "interactive", "modern-css"],
  "author": "Équipe Dev"
}' > config.json

git add config.json
git commit -m "Mise à jour config vers v2.0 avec nouvelles features"

# REBASE au lieu de merge
git checkout feature/javascript-interactif
git rebase main
```

**Comparaison visuelle avant/après rebase :**

**AVANT rebase :**
```
main
  ↓
  * Mise à jour config vers v2.0
  ↓
  * Résolution conflit: fusion contenu main + footer
  │
feature/javascript-interactif
  ↓
  * JavaScript interactif: click header + scroll effects
  ↓
  * (point de divergence)
```

**APRÈS rebase :**
```
main
  ↓
  * Mise à jour config vers v2.0
  ↓
  * Résolution conflit: fusion contenu main + footer
  │
feature/javascript-interactif (HEAD)
  ↓
  * JavaScript interactif: click header + scroll effects (REJOUÉ)
  ↓
  * Mise à jour config vers v2.0
```

```bash
# Retour sur main pour intégration propre
git checkout main
git merge feature/javascript-interactif  # Fast-forward grâce au rebase !
```

**Historique final linéaire :**
```
main (HEAD)
  ↓
  * JavaScript interactif: click header + scroll effects
  ↓
  * Mise à jour config vers v2.0
  ↓
  * Résolution conflit: fusion contenu main + footer
  ↓
  * [historique précédent...]
```

[⬆️ retour à la table des matières](#table-des-matieres)
<br/>

---

<a name="resume-commandes"></a>
## **Résumé des commandes**
---

### **Commandes complètes du tutoriel**

1. **Projet et initialisation** :
   ```bash
   mkdir projet-merge-rebase-demo && cd projet-merge-rebase-demo
   git init
   # ... création fichiers ...
   git add . && git commit -m "Version initiale du site web"
   ```

2. **Gestion des branches** :
   ```bash
   git checkout -b feature/nom-feature    # Créer branche
   # ... développement ...
   git checkout main                      # Retour main
   ```

### **Commandes Merge**

3. **Merge Fast-forward** :
   ```bash
   git merge feature/header-ameliore      # Fusion directe
   ```

4. **Merge Three-way** :
   ```bash
   git merge feature/styles-avances       # Crée commit de fusion
   ```

5. **Résolution de conflits** :
   ```bash
   git merge feature/footer              # Conflit détecté
   # -> Éditer fichiers en conflit
   git add fichier-resolu.html
   git commit -m "Résolution conflit"
   ```

### **Commandes Rebase**

6. **Rebase standard** :
   ```bash
   git checkout feature/javascript-interactif
   git rebase main                       # Rejouer commits sur main
   git checkout main
   git merge feature/javascript-interactif  # Fast-forward
   ```

### **Commandes de visualisation**
```bash
git log --oneline --graph             # Voir historique graphique
git log --oneline                     # Historique simplifié
git branch                            # Lister branches
git status                            # État du dépôt
```

[⬆️ retour à la table des matières](#table-des-matieres)
<br/>

---

<a name="conclusion"></a>
## **Conclusion**
---

**Félicitations !** Vous avez maîtrisé les concepts fondamentaux de **Git Merge vs Rebase** avec un projet web complet.

**Ce que vous avez appris :**
- Différences conceptuelles entre merge et rebase  
- Types de merge (fast-forward, three-way, avec conflits)
- Résolution de conflits de fusion
- Utilisation pratique de rebase pour un historique propre
- Visualisation de l'impact sur l'historique Git

**Récapitulatif des stratégies :**

**Utilisez MERGE quand :**
- Vous intégrez des branches publiques/partagées
- Vous voulez préserver l'historique réel de développement  
- Vous travaillez en équipe sur les mêmes branches
- Vous voulez voir clairement où les branches ont divergé

**Utilisez REBASE quand :**
- Vous nettoyez l'historique de vos branches locales
- Vous voulez un historique linéaire et propre
- Vous préparez une branche avant de la partager
- Vous travaillez seul sur une fonctionnalité

**Règles d'or à retenir :**
- **Ne jamais rebaser des branches publiques** 
- **Merge** = sûr mais historique complexe
- **Rebase** = historique propre mais réécrit l'histoire
- **Fast-forward** = meilleur des deux mondes quand possible

**Workflow recommandé :**
1. Développer sur branche feature locale
2. Rebase sur main pour nettoyer avant partage
3. Merge pour intégrer en production
4. Supprimer branches fusionnées

Vous pouvez désormais choisir la stratégie appropriée selon le contexte et maintenir un historique Git professionnel !

[⬆️ retour à la table des matières](#table-des-matieres)
<br/>

---