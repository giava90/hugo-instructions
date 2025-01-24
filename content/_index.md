---
title: Instructions for Creating Your First Website
---

# Instructions for Creating Your First Website

## Preliminaries
0. **Make sure to have read my first email**
Here a [link](preparation/) for those of you that have might missed it out
1. **Create a folder for websites**  
   ```bash
   mkdir websites
   ```
2. **Move into the folder**  
   ```bash
   cd websites
   ```
3. **Create the website**  
   ```bash
   hugo new site name-of-the-site
   ```
4. **Move into the website folder**  
   ```bash
   cd name-of-the-site
   ```

## Choosing a Theme

1. Browse themes on [Hugo Themes](https://themes.gohugo.io/).
2. Click on a theme you like.
3. Read the theme's page for specific instructions. If instructions are provided (e.g., **Hugoplate**), follow them.
4. If no special instructions are provided:
   - Click on **Download** to go to the theme's GitHub page.
   - Read the README (if available) and check for an example site.
   - If you like the theme and it looks manageable, clone the repository:  
     ```bash
     cd themes
     git clone repo-url
     dir  # or ls, to confirm the repo is present
     ```

## Adding Content to Your Website

1. Follow the theme's instructions to set up the site. If no instructions are provided, here are some basic steps:
   - Create two folders and a landing page:  
     ```bash
     cd ../content
     mkdir about
     mkdir projects
     touch _index.md
     ```
   - Open `_index.md` and add the following (using markdown):  
     ```markdown
     # Welcome to My Website
     This is the homepage. Add a banner, a title, and some information here.
     ```
   - Create two pages:  
     - **About page:**  
       ```bash
       cd about
       touch index.md
       ```
       Open `index.md` and add:  
       ```markdown
       ## About Me
       Write a brief description about yourself here. You can also add links to your projects.
       ```
     - **Projects page:**  
       ```bash
       cd ../projects
       touch index.md
       ```
       Open `index.md` and add:  
       ```markdown
       ## My Projects
       Add information about your projects here. Include links if applicable.
       ```
2. Add links to the **Projects** and **About** pages on your landing page:
   - **Easy method:** Edit `content/_index.md` and add links manually.
   - **Advanced method:** Update your `hugo.toml` file:  
     ```toml
     [menu]
     [[menu.main]]
         name = "Home"
         url = "/"
         weight = 1

     [[menu.main]]
         name = "Projects"
         url = "/projects/"
         weight = 2

     [[menu.main]]
         name = "About"
         url = "/about/"
         weight = 3
     ```

## Checking the Website Locally

1. Update the `hugo.toml` file:
   - Change the `title` to your website's title.
   - Update the `theme` to the name of the folder where you cloned the theme.
2. Launch the website locally:  
   ```bash
   hugo server
   ```
3. Open your browser and verify the website.

## Connecting to GitHub

1. **Initialize a Git repository:**  
   ```bash
   git init
   ```
2. **Add the theme as a submodule:**  
   ```bash
   git submodule add url-of-the-theme-from-GitHub themes/name-you-used-locally
   ```
3. **Stage and commit all files:**  
   ```bash
   git add .
   git commit -m "My first commit with the initial files"
   ```

### Synchronizing with GitHub

1. Go to GitHub and create a new repository:
   - Log in and navigate to your homepage.
   - Click the top-right button to create a new repo.
   - Name the repo (e.g., `mainwood` or `giacomo-vaccario`).
   - Leave other options as default, then click **Create**.
2. Copy the repo's URL.
3. Add the GitHub repo as the remote:  
   ```bash
   git remote add origin url-of-the-newly-created-repo
   ```
   Verify it worked:  
   ```bash
   git remote -v
   ```
4. Push local changes to GitHub:  
   ```bash
   git push origin main  # or simply git push
   ```

### Verify Online

1. Refresh your GitHub repository page.
2. Confirm your website's files are visible online.

# Deploy Your Website Using GitHub Actions

This guide will show you how to automate the deployment of your Hugo website to GitHub Pages using GitHub Actions.

---

## What is GitHub Actions Workflow?

GitHub Actions allows you to automate tasks, such as building and deploying your Hugo website. The website will be hosted on a branch called `gh-pages` of your repository.  
**What is a branch?**  
A branch is a version of your repository. The `gh-pages` branch is commonly used to host files for websites or documentation.

---

## Steps to Set Up Deployment

### 1. Create the `gh-pages` Branch
Run the following command to create the `gh-pages` branch:  
```bash
git branch gh-pages
```

### 2. Create Folders for the Workflow
1. Inside your website directory, create a `.github` folder:  
   ```bash
   mkdir .github
   ```
2. Create a subfolder named `workflows`:  
   ```bash
   cd .github
   mkdir workflows
   ```

### 3. Create the Workflow File
1. Inside the `workflows` folder, create a file named `deploy_hugo.yml`:  
   ```bash
   touch deploy_hugo.yml
   ```
2. Open the file and copy-paste the following code:  

   ```yaml
   name: Deploy Hugo Site to GitHub Pages

   on:
     push:
       branches:
         - main

   jobs:
     build:
       runs-on: ubuntu-latest

       steps:
       # Step 1: Check out the repository
       - name: Checkout code
         uses: actions/checkout@v2

       # Step 2: Set up Hugo
       - name: Set up Hugo
         uses: peaceiris/actions-hugo@v2
         with:
           hugo-version: '0.112.0' # Specify the Hugo version

       # Step 3: Build the site
       - name: Build the Hugo site
         run: hugo --minify --destination docs

       # Step 4: Deploy to GitHub Pages
       - name: Deploy to GitHub Pages
         uses: peaceiris/actions-gh-pages@v3
         with:
           publish_dir: ./public # This is the folder Hugo generates the site in
           github_token: ${{ secrets.GITHUB_TOKEN }} # Token to authenticate with GitHub
   ```

3. Save the file and review the steps in the workflow.

---

## 4. Commit and Push the Changes

1. Add the new files to your repository:  
   ```bash
   git add .
   ```
2. Commit the changes:  
   ```bash
   git commit -m "Deploy instructions for GitHub Actions"
   ```
3. Push the changes to GitHub:  
   ```bash
   git push
   ```

---

## 5. Trigger the Workflow

The workflow is triggered automatically on a `git push`. Follow these steps to monitor it:
1. Go to your repository on GitHub.
2. Click on the **Actions** tab.
3. Look for a workflow with the commit message "Deploy instructions ...".
4. Click on it twice to view the progress of the workflow.  
   The steps might take 10–50 seconds depending on your workflow's complexity.

---

## Troubleshooting Common Issues

If the deployment fails, here are common fixes:

### **1. Update Pages Settings**
1. Go to your repository on GitHub.
2. Click **Settings** (top-right menu).
3. Under **Pages** (left menu):
   - **Build and deployment**: Ensure the source is set to `Deploy from branch`.
   - **Branch**: Select `gh-pages`.

### **2. Update Actions Settings**
1. Go to your repository on GitHub.
2. Click **Settings** (top-right menu).
3. Under **Actions** (left menu):
   - Click **General**.
   - Scroll to the bottom and:
     - Allow "Read and write permissions."
     - Ensure actions are allowed to run.

### **3. Check Environment Settings**
1. Go to your repository on GitHub.
2. Click **Settings** (top-right menu).
3. Under **Environment** (left menu):
   - Ensure there are no restrictions.

---

After following these steps, your Hugo website should be deployed on GitHub Pages! 🎉
```