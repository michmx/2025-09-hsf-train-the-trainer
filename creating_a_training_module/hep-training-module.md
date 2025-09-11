# Creating Training Modules with Jupyter Book

This chapter shows trainers in the HEP community how to develop and publish a self‑contained training module 
using Jupyter Book and the cookiecutter template. 

## Prerequisites
- Python 3.9+ with a virtual environment (venv or conda/mamba).
- `jupyter-book` and related tools (see this repository’s `requirements.txt`).
- Optional but recommended: `cookiecutter` 

```bash
# Environment (example with venv)
python -m venv venv && source venv/bin/activate
pip install -r requirements.txt
pip install cookiecutter 
```

## Scaffold with Cookiecutter

Use the official template to generate a new book repo (for a fresh module):

```bash
cookiecutter gh:executablebooks/cookiecutter-jupyter-book
# Answer prompts (project_name, author, etc.)
cd your-book-name
```

Key files: `_config.yml` (book metadata), `_toc.yml` (structure), content pages (`.md`, `.ipynb`).

## Authoring Content 

- Prefer MyST Markdown for lessons; use notebooks when execution is required.
- Keep one concept per page; add exercises with solutions in collapsible sections.
- Cite datasets, software, and papers; keep a `references.bib` and use `{cite}`.

````md
```{admonition} Learning objectives
- Understand ROOT I/O with `uproot`
- Plot with `matplotlib`/`mplhep`
```
````

## Environments and Reproducibility

For modules using ROOT/Scikit‑HEP, provide an environment and pin versions.

```yaml
# environment.yml (conda)
name: hep-training
channels: [conda-forge]
dependencies:
  - python=3.11
  - jupyter
  - jupyter-book
  - matplotlib
  - numpy
  - uproot
  - awkward
  - mplhep
  - root-base  # optional; Linux/macOS via conda-forge
```

Commit `environment.yml` (or `requirements.txt`) and show learners how to activate it.

## Build, Check, and Preview
```bash
jupyter-book clean .
jupyter-book build -W --keep-going .   # fail on warnings
python -m http.server -d _build/html 8000
```
Open http://localhost:8000 to preview.

## Publish
- GitHub Pages (manual): `ghp-import -n -p -f _build/html`
- Or enable the CI option in cookiecutter and push; Actions will build and publish to `gh-pages`.

## Trainer Tips
- Start each chapter with objectives and time estimates; end with a summary and checks for understanding.
- Keep runtime under 2–3 minutes per executed page; cache heavy outputs.
- Provide solutions and a short “further reading” list tailored to HEP.
