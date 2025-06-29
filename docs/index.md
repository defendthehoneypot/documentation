# Build an MkDocs Material Site Using GitHub Only (No Local Install)  

This guide walks you through building and publishing a Material for MkDocs site entirely within GitHub, using GitHub Actions.  

---  

## Step 1: Create a New GitHub Repository  

1. Go to https://github.com/new  
2. Name it something like `Documentation`  
3. Click "Create repository"  

---  

## Step 2: Add `mkdocs.yml` and Docs Folder  

Create the following files using GitHub’s web interface:  

### `mkdocs.yml`  

```yaml  
site_name: My Documentation Site  
theme:  
  name: material  
```  

You can customize this later. See: https://squidfunk.github.io/mkdocs-material/setup/  

You can also view a working example of an `mkdocs.yml` file here:  
- https://github.com/defendthehoneypot/documentation/blob/main/mkdocs.yml  
- Ensure you change the following lines to match your site before commiting  
```
site_url: https://defendthehoneypot.github.io/documentation
repo_url: https://github.com/defendthehoneypot/documentation
```  

Live site example:  
- https://defendthehoneypot.github.io/documentation/  

---  

### `docs/index.md`  

Create a folder named `docs/` and add a file named `index.md`:  

```markdown  
# Welcome to My Docs  

This site is built with [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/).  
```  

---  

## Step 3: Create Branch for GitHub Pages  
`gh-pages`  

---  

## Step 4: Add GitHub Actions Workflow  

Create this file in your repository:  

`.github/workflows/gh-pages.yml`  

```yaml  
name: Deploy MkDocs site to GitHub Pages  

on:  
  push:  
    branches:  
      - main  # Or 'master', depending on your default branch  

permissions:  
  contents: write  # Required to allow pushing to gh-pages  

jobs:  
  deploy:  
    runs-on: ubuntu-latest  

    steps:  
      - uses: actions/checkout@v4  

      - name: Set up Python  
        uses: actions/setup-python@v5  
        with:  
          python-version: '3.x'  

      - name: Install dependencies  
        run: pip install mkdocs-material  

      - name: Deploy to GitHub Pages  
        run: mkdocs gh-deploy --force  
```  

---  

## Step 5: Allow GitHub Actions to Push to the Repository  

1. Go to your repo → Settings → Actions → General  
2. Scroll to "Workflow permissions"  
3. Select:  
   [x] Read and write permissions  
4. Click "Save"  

> This gives the Action permission to push to the `gh-pages` branch.  

---  

## Step 6: Configure GitHub Pages  

1. Go to Settings → Pages  
2. Under Source, select:  
   - Branch: `gh-pages`  
   - Folder: `/ (root)`  
3. Click "Save"  

---  

## Step 7: Understanding the `nav` Section (Optional)  

The `nav` section in `mkdocs.yml` lets you define a custom navigation structure for your site.  

### Without `nav` (automatic)  

If you do **not** specify `nav`, MkDocs will automatically create navigation by scanning your `docs/` folder alphabetically.  

Example:  

```yaml  
site_name: My Docs  
theme:  
  name: material  
```  

If your `docs/` folder has:  
```
docs/  
  index.md  
  about.md  
  faq.md  
```  

MkDocs will display:  
- Home  
- About  
- Faq  

Sorted alphabetically (or by file system order).  

---  

### With `nav` (manual control)  

To control order and display names, use `nav`:  

```yaml  
site_name: My Docs  
theme:  
  name: material  

nav:  
  - Home: index.md  
  - About This Site: about.md  
  - FAQ: faq.md  
```  

This lets you:  
- Change link titles  
- Set custom order  
- Nest pages into groups  

### When to use `nav`  

Use `nav` if:  
- You want to control page order  
- You want custom link names  
- You want grouped or nested sections  

Leave it out if:  
- Your site is very small  
- You’re okay with alphabetical order  

---  

## Step 8: Your Site Is Live  

After you push to `main`, GitHub Actions will build and deploy your site.  

Visit:  
```
https://your-username.github.io/your-repo/  
```  

(Replace with your GitHub username and repo name)  

---  

## Step 8: Updating Content  

Any time you update `docs/` or `mkdocs.yml`, GitHub Actions will automatically redeploy your site.  

---  

## Resources  

- MkDocs: https://www.mkdocs.org/  
- MkDocs Material: https://squidfunk.github.io/mkdocs-material/  

