#  CI/CD Deployment with GitHub Actions & Vite + React

##  Goal

Automate the build and deployment of a React application (created with Vite) using GitHub Actions and GitHub Pages.

---
### 🛠️ Technologies Utilisées
- React

- Vite

- GitHub Actions

- gh-pages

### 📂 Arborescence Clé

```
hello_world_app/
├── dist/                # Dossier généré par Vite
├── src/                 # Composants React
├── .github/workflows/  # Contient le fichier de déploiement CI/CD
│   └── deploy.yml
├── vite.config.js       # Configuration du chemin de base
├── package.json         # Scripts de build et deploy
```

## 🛠️ Project Setup

### 1. Create the React App

```
npm create vite@latest hello_world_app -- --template react
cd hello_world_app
npm install
```
### 2.  Initialisation Git & Dépôt GitHub
```
git init
git remote add origin https://github.com/DJOUMESSI2004/epsi_ci_cd_project.git
git add .
git commit -m "Initial commit"
git push -u origin master

```

### 2.  Initialisation Git & Dépôt GitHub
Pour s’assurer que les ressources se chargent correctement depuis GitHub Pages :

```
// vite.config.js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  base: '/epsi_ci_cd_project/', // Chemin du dépôt GitHub
  plugins: [react()],
})

```

### 4. Installation des dépendances nécessaires au déploiement

```
npm install gh-pages --save-dev

```
Ajout dans package.json :

```
"scripts": {
  "predeploy": "npm run build",
  "deploy": "gh-pages -d dist"
}

```
### 5. Configuration du Workflow GitHub Actions
Chemin : .github/workflows/deploy.yml
```
name: CI/CD Pipeline

on:
  push:
    branches:
      - master

jobs:
  build-deploy:
    runs-on: ubuntu-latest

    permissions:
      contents: write 

    steps:
      - name: Checkout le code
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Installer les dépendances
        run: npm install

      - name: Lancer les tests
        run: echo "No tests yet" && exit 0

      - name: Build de l'app
        run: npm run build

      - name: Configurer Git
        run: |
          git config user.name "myusername"
          git config user.email "myemail@gmail.com"

      - name: Déployer sur GitHub Pages
        run: npm run deploy
        env:
          GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### ❌ Erreurs rencontrées & ✅ Solutions

**Cause** : Aucun script de test défini.

**Solution** : Remplacer par une commande vide dans le workflow :
```
run: echo "No tests yet" && exit 0

```
### ❌ Erreur : ENOENT: no such file or directory, stat 'build'

**Cause** : Vite génère un dossier dist et non build.

**Solution** : Dans package.json :

```
"deploy": "gh-pages -d dist"

```

### ❌ Erreur : empty ident name (problème d'identité git)
**Cause** : L’action GitHub n’a pas d’identité utilisateur.

**Solution** : Ajouter une étape de config Git dans le workflow :

```
git config user.name "Mon Nom"
git config user.email "monmail@example.com"

```

### ❌ Erreur : Permission denied to github-actions[bot]
**Cause** : Mauvaise URL de déploiement.

**Solution*** : Ajouter le token d’auth dans l’URL :

```
gh-pages -d dist -r https://x-access-token:${GH_TOKEN}@github.com/MON_USERNAME/NOM_DU_DEPOT.git

```

### ❌ Erreur : Bad substitution → GH_TOKEN

**Cause** : Mauvais usage de ${{ secrets.GITHUB_TOKEN }} dans un script shell.

**Solution** : Passer la variable d’environnement correctement via env: :

```
env:
  GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}

```

### ❌ Erreur 404 sur les fichiers CSS/JS après déploiement
**Cause** : Mauvais chemin de base.

**Solution** : Bien définir base: '/nom_du_depot/' dans vite.config.js.

### ✅ Résultat final
- ✅ L’application est buildée automatiquement après chaque push.

- ✅ Le déploiement est fait via GitHub Actions vers GitHub Pages.

- ✅ Le site est accessible à l'adresse :
 🔗 https://djoumessi2004.github.io/epsi_ci_cd_project/









