# Continuous Integration: Test and Deploy to GitHub Pages

This chapter shows how to use GitHub Actions to validate and publish your Jupyter Book. 
It targets HEP trainers building modules with this repository layout.

## Start a Git repository in the book directory

```bash
cd YOUR_BOOK_DIRECTORY
git init
```

This will create `.git/` in your book directory.

```{warning}
Never modify the `.git/` directory! Use git commands to manage your repository. 
```

Create a `.gitignore` file to exclude unnecessary files. For example;

```text 
# Content of .gitignore
_build/
__pycache__/
```

Add everything and make the initial commit:

```bash 
git add .
git commit -m "Initial commit"
```

## Create a GitHub repository

Create a new repository on GitHub (do not initialize with README, .gitignore, or license).

Follow the instructions to add your remote and push the initial commit. Basically, something like:

```bash
git remote add origin git@github.com:YOUR_USERNAME/YOUR_REPO.git
git branch -M main
git push -u origin main
```

In Repository Settings / Pages, set “Build and deployment” to “GitHub Actions”.
 

## Recommended Workflow (Actions)

Ensure `requirements.txt` includes `jupyter-book` (and any dependencies for executing the book).

Create `.github/workflows/book.yml`:

```yaml
name: jupyter-book

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: true

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.11'
      - name: Install dependencies
        run: |
          python -m pip install -U pip
          pip install -r requirements.txt
      - name: Build Jupyter Book (treat warnings as errors)
        working-directory: YOUR_BOOK_DIRECTORY
        run: |
          jupyter-book build -W --keep-going .
      - name: Upload Pages artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: YOUR_BOOK_DIRECTORY/_build/html

  deploy:
    if: github.ref == 'refs/heads/main'
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - id: deployment
        uses: actions/deploy-pages@v4
```

Commit and push this file to `main`. Look at the Actions tab to see the workflow running.

And done! Every push to `main` will rebuild and deploy your book. 

```{tip}
You can customize the workflow as needed, e.g. to rebuild the book daliy for checking 
that the material is up to date with the most recent package versions.
```


## PR Testing and Link Checks
- The workflow builds on every PR; failures block merges.
- Optional link check job:

```yaml
  linkcheck:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with: { python-version: '3.11' }
      - run: |
          pip install -r requirements.txt
          jupyter-book build --builder linkcheck YOUR_BOOK_DIRECTORY
```


