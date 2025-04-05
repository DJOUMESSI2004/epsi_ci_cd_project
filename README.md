#  CI/CD Deployment with GitHub Actions & Vite + React

##  Goal

Automate the build and deployment of a React application (created with Vite) using GitHub Actions and GitHub Pages.

---

## 🛠️ Project Setup

### 1. Create the React App

```bash
npm create vite@latest hello_world_app -- --template react
cd hello_world_app
npm install
2. Initialize Git and Connect to GitHub
bash
Copy
Edit
git init
git remote add origin https://github.com/YOUR_USERNAME/your-repo-name.git
git add .
git commit -m "Initial commit"
git push -u origin master
3. Configure Vite for GitHub Pages
Edit vite.config.js:

js
Copy
Edit
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  base: '/your-repo-name/', // Use your GitHub repo name here
  plugins: [react()],
})
4. Setup Deployment Script
Install gh-pages:

bash
Copy
Edit
npm install --save-dev gh-pages
In package.json, add:

json
Copy
Edit
"scripts": {
  "dev": "vite",
  "build": "vite build",
  "preview": "vite preview",
  "predeploy": "npm run build",
  "deploy": "gh-pages -d dist"
}
⚙️ GitHub Actions Workflow
Create the file .github/workflows/deploy.yml:

yaml
Copy
Edit
name: CI/CD Pipeline

on:
  push:
    branches:
      - master

jobs:
  build-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: echo "No tests yet" && exit 0

      - name: Build the app
        run: npm run build

      - name: Configure Git
        run: |
          git config user.name "YOUR_NAME"
          git config user.email "your-email@example.com"

      - name: Deploy to GitHub Pages
        run: |
          npm run deploy -- -u "YOUR_NAME <your-email@example.com>" -r https://x-access-token:${{ secrets.GITHUB_TOKEN }}@github.com/YOUR_USERNAME/your-repo-name.git
        env:
          GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
 Common Errors & Solutions
 Missing script: "test"
 Solution: No test defined yet

yaml
Copy
Edit
run: echo "No tests yet" && exit 0
 ENOENT: no such file or directory, stat 'build'
 Solution: Use dist for Vite

json
Copy
Edit
"deploy": "gh-pages -d dist"
 empty ident name or Permission denied
 Solution: Set Git identity + use correct GitHub token:

yaml
Copy
Edit
git config user.name "Your Name"
git config user.email "your-email@example.com"
bash
Copy
Edit
gh-pages -d dist -r https://x-access-token:${GH_TOKEN}@github.com/YOUR_USERNAME/your-repo-name.git
 Assets 404 on GitHub Pages
 Solution: Ensure vite.config.js contains:

js
Copy
Edit
base: '/your-repo-name/'
 Result
Your app is automatically:

Built on every push to master

Deployed to GitHub Pages at:
 https://YOUR_USERNAME.github.io/your-repo-name/

 Resources
🔗 GitHub Repository

📄 This README
