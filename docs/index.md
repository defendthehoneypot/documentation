# Build a Zensical Site Using GitHub Only (No Local Install)

This guide walks you through building and publishing a Zensical site entirely within GitHub, using GitHub Actions.

---

## What is Zensical?

[Zensical](https://zensical.org) is a modern static site generator (SSG) that replaces MkDocs + Material for MkDocs. Built by the team behind Material for MkDocs, Zensical is the next generation solution for documentation sites, offering:

- **Faster builds** (often 5x faster than MkDocs)
- **Modern architecture** designed for docs-as-code workflows
- **Easy migration** from MkDocs (uses `mkdocs.yml` and compatible syntax)
- **Open source & free** with optional commercial support

As of November 2025, Material for MkDocs has moved to maintenance mode, with active development focusing on Zensical.

---

## Step 1: Create a New GitHub Repository

1. Go to https://github.com/new
2. Name it something like `Documentation`
3. Click "Create repository"

---

## Step 2: Create Branch for GitHub Pages

The branch is required to write the html data for displaying the Zensical site.

`gh-pages`

---

## Step 3: Add `zensical.toml` and Docs Folder

Create the following files using GitHub's web interface:

### `zensical.toml`

```toml
site_name = "My Documentation Site"
theme = "material"
```

You can customize this later. See: https://zensical.org/docs/

You can also view a working example of a `zensical.toml` file here:
- https://github.com/defendthehoneypot/documentation/blob/main/zensical.toml
- Ensure you change the following lines to match your site before commiting
```toml
site_url = "https://defendthehoneypot.github.io/documentation"
repo_url = "https://github.com/defendthehoneypot/documentation"
```

Live site example:
- https://defendthehoneypot.github.io/documentation/

### `docs/index.md`

Create a folder named `docs/` and add a file named `index.md`:

```markdown
# Welcome to My Docs

This site is built with [Zensical](https://zensical.org).
```

---

## Step 4: Add GitHub Actions Workflow

Create this file in your repository:

`.github/workflows/gh-pages.yml`

```yaml
name: Deploy Zensical site to GitHub Pages

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

      - name: Install Zensical
        run: pip install zensical

      - name: Deploy to GitHub Pages
        run: zensical build --output gh-pages
        
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./gh-pages
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

The `nav` section in `zensical.toml` lets you define a custom navigation structure for your site.

### Without `nav` (automatic)

If you do **not** specify `nav`, Zensical will automatically create navigation by scanning your `docs/` folder alphabetically.

Example:

```toml
site_name = "My Docs"
theme = "material"
```

If your `docs/` folder has:
```
docs/
  index.md
  about.md
  faq.md
```

Zensical will display:
- Home
- About
- Faq

Sorted alphabetically (or by file system order).

### With `nav` (manual control)

To control order and display names, use `nav`:

```toml
site_name = "My Docs"
theme = "material"

[nav]
"Home" = "index.md"
"About This Site" = "about.md"
"FAQ" = "faq.md"
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
- You're okay with alphabetical order

---

## Step 8: Your Site Is Live

After you push to `main`, GitHub Actions will build and deploy your site.

Visit:
```
https://your-username.github.io/your-repo/
```

(Replace with your GitHub username and repo name)

---

## Step 9: Updating Content

Any time you update `docs/` or `zensical.toml`, GitHub Actions will automatically redeploy your site.

---

## Resources

- Zensical: https://zensical.org
- Zensical Documentation: https://zensical.org/docs/
- GitHub Pages: https://pages.github.com/
