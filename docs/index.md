# 🌐 Build an MkDocs Material Site Using GitHub Only (No Local Install)

This guide walks you through building and publishing a Material for MkDocs site **entirely within GitHub**, using **GitHub Actions**.

---

## ✅ Step 1: Create a New GitHub Repository

1. Go to https://github.com/new  
2. Name it something like `my-docs-site`  
3. Click **Create repository**

---

## 📄 Step 2: Add `mkdocs.yml` and Docs Folder

Create the following files using GitHub’s web interface:

### `mkdocs.yml`

```yaml
site_name: My Documentation Site
theme:
  name: material
```

Customize as needed. See: https://squidfunk.github.io/mkdocs-material/setup/

---

### `docs/index.md`

Create a folder named `docs/` and add a file named `index.md`:

```markdown
# Welcome to My Docs

This site is built with [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/).
```

---

## 🤖 Step 3: Add GitHub Actions Workflow

Create this file in your repository:

`.github/workflows/gh-pages.yml`

```yaml
name: Deploy MkDocs site to GitHub Pages

on:
  push:
    branches:
      - main  # Or 'master', depending on your default branch

permissions:
  contents: write

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

## ⚙️ Step 4: Configure GitHub Pages

1. Go to **Settings** → **Pages**
2. Under **Source**, select:
   - Branch: `gh-pages`
   - Folder: `/ (root)`
3. Click **Save**

---

## 🌍 Your Site Is Live!

After you push to `main`, GitHub Actions will build and deploy your site.

Visit:  
```
https://your-username.github.io/your-repo/
```

(Replace with your GitHub username and repo name)

---

## 📝 Updating Content

Any time you update `docs/` or `mkdocs.yml`, GitHub Actions will automatically redeploy your site.

---

## 🔗 Resources

- MkDocs: https://www.mkdocs.org/
- MkDocs Material: https://squidfunk.github.io/mkdocs-material/
