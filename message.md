# TP : Déployer un site web avec GitHub Actions et GitHub Pages

## Objectif
Dans ce TP, vous apprendrez à configurer un pipeline d'intégration continue (CI) pour automatiser le déploiement d'un site web statique avec GitHub Actions sur GitHub Pages. L'objectif est de comprendre les bases de l'automatisation des déploiements à travers une plateforme CI/CD et d'utiliser GitHub Pages pour héberger un site.

## Contexte
GitHub Actions est une fonctionnalité puissante de GitHub permettant de définir des workflows pour automatiser des tâches comme les tests, la compilation ou le déploiement. Couplé à GitHub Pages, il permet d'automatiser le déploiement de sites web statiques (HTML/CSS/JavaScript) directement à partir de votre dépôt.

## Prérequis
- Compte GitHub
- Connaissance de base en HTML/CSS/JavaScript
- Notions de Git et GitHub (commits, push, pull requests)
- Notions de base sur les systèmes CI/CD

## Étape 1 : Création du dépôt et du site web

1. **Création d’un dépôt GitHub :**
   - Rendez-vous sur [GitHub](https://github.com) et créez un nouveau dépôt public. Nommez-le par exemple `tp-github-pages`.
   - Initialisez le dépôt avec un fichier `README.md` et sélectionnez l’option `.gitignore` pour les fichiers de log.

2. **Structure du site web :**
   - Clonez le dépôt localement :
     ```bash
     git clone https://github.com/<votre-utilisateur>/tp-github-pages.git
     cd tp-github-pages
     ```
   - Créez un fichier `index.html` à la racine du projet avec ce contenu :
     ```html
     <!DOCTYPE html>
     <html lang="fr">
     <head>
         <meta charset="UTF-8">
         <meta name="viewport" content="width=device-width, initial-scale=1.0">
         <title>Mon TP GitHub Pages</title>
         <style>
           body {
             font-family: Arial, sans-serif;
             text-align: center;
             padding-top: 50px;
           }
         </style>
     </head>
     <body>
         <h1>Bienvenue sur mon site déployé avec GitHub Pages</h1>
         <p>Ceci est un exemple de site statique déployé via un pipeline GitHub Actions.</p>
     </body>
     </html>
     ```
   - Ensuite, ajoutez, commitez et poussez ce fichier :
     ```bash
     git add index.html
     git commit -m "Ajout du fichier index.html"
     git push origin main
     ```

## Étape 2 : Activer GitHub Pages

1. **Configuration de GitHub Pages :**
   - Allez dans l'onglet `Settings` de votre dépôt GitHub, puis dans la section `Pages`.
   - Sous `Source`, choisissez la branche `main` et le dossier `root`, puis cliquez sur `Save`.
   - GitHub Pages va générer une URL (généralement au format `https://<votre-utilisateur>.github.io/tp-github-pages`), qui sera utilisée pour accéder à votre site.

## Étape 3 : Créer le workflow GitHub Actions

1. **Création du fichier de workflow :**
   - Créez un dossier `.github/workflows` dans votre dépôt :
     ```bash
     mkdir -p .github/workflows
     ```
   - Dans ce dossier, créez un fichier `deploy.yml` avec le contenu suivant :
     ```yaml
     name: Déploiement sur GitHub Pages

     # Déclenche le workflow lors d'un push sur la branche main
     on:
       push:
         branches:
           - main

     jobs:
       deploy:
         runs-on: ubuntu-latest

         steps:
           # Checkout du dépôt
           - name: Checkout code
             uses: actions/checkout@v2

           # Déploiement sur GitHub Pages
           - name: Deploy to GitHub Pages
             uses: peaceiris/actions-gh-pages@v3
             with:
               github_token: ${{ secrets.GITHUB_TOKEN }}
               publish_dir: ./ # Répertoire à publier
     ```

2. **Explication du workflow :**
   - `on: push`: Le workflow se déclenche à chaque `push` sur la branche `main`.
   - `jobs`: Une série d'étapes sont exécutées.
   - `uses: actions/checkout@v2`: Cette étape récupère le code du dépôt.
   - `uses: peaceiris/actions-gh-pages@v3`: Cette action publie le site sur GitHub Pages, en utilisant le répertoire racine `./` comme contenu à déployer.

3. **Committer et pousser le workflow :**
   - Après avoir créé le fichier `deploy.yml`, ajoutez, commitez et poussez :
     ```bash
     git add .github/workflows/deploy.yml
     git commit -m "Ajout du workflow de déploiement GitHub Pages"
     git push origin main
     ```

## Étape 4 : Vérification du déploiement

1. **Vérification dans l’onglet `Actions` :**
   - Accédez à l'onglet `Actions` dans GitHub. Vous devriez voir le workflow en cours d'exécution.
   - Une fois terminé, rendez-vous sur l'URL GitHub Pages fournie pour vérifier que votre site est déployé.

## Étape 5 : Modification et redéploiement

1. **Modification du site :**
   - Modifiez le fichier `index.html` en changeant le texte ou en ajoutant un élément HTML (par exemple, un nouveau paragraphe).
   - Après avoir fait une modification, poussez les changements :
     ```bash
     git add index.html
     git commit -m "Modification du contenu du site"
     git push origin main
     ```

2. **Vérification du redéploiement :**
   - Accédez à nouveau à l'onglet `Actions` pour suivre l'exécution du workflow.
   - Une fois l'exécution terminée, rafraîchissez l'URL du site pour voir vos modifications.

---

## Conclusion
À travers ce TP, vous avez appris à utiliser GitHub Actions pour automatiser le déploiement d’un site web statique via GitHub Pages. Ce processus CI/CD simple permet de publier du contenu automatiquement à chaque mise à jour du code. Vous pouvez désormais adapter ce workflow pour des projets plus complexes, en intégrant des étapes supplémentaires comme les tests ou la génération dynamique de contenu.

### Pour aller plus loin :
- Ajoutez des étapes pour exécuter des tests unitaires avant le déploiement.
- Explorez d'autres actions GitHub pour étendre les capacités de votre pipeline CI/CD.
