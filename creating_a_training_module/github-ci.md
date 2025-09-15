# Continuous Integration: Test and Deploy to GitHub Pages

This chapter shows how to use GitHub Actions to validate and publish your Jupyter Book. 
It targets HEP trainers building modules with this repository layout.

## Setup
- Push your repository to GitHub.
- In Repository Settings / Pages, set “Build and deployment” to “GitHub Actions”.
- Ensure `requirements.txt` includes `jupyter-book` (and any dependencies for executing the book).

## Recommended Workflow (Actions)
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


