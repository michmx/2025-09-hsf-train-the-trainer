# Creating Training Modules with Jupyter Book

This chapter shows trainers in the HEP community how to develop and publish a self‑contained training module 
using Jupyter Book and the cookiecutter template. 

## Installation

We require 

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

## Adding Content

Add exercises with solutions in collapsible sections.

````md
```{admonition} Learning objectives
- Understand ROOT I/O with `uproot`
- Plot with `matplotlib`/`mplhep`
```
````

Cite datasets, software, and papers; keep a `references.bib` and use `{cite}`.

## Environments and Reproducibility

For modules using packages, prefer a Python virtual environment (venv) and a pinned `requirements.txt`.

```bash
# Create and activate a venv
python -m venv venv
source venv/bin/activate

# Install from your requirements file
pip install -r creating_a_training_module/requirements.txt

# (Optional) capture an exact lock for reproducibility
pip freeze > creating_a_training_module/requirements.lock
```

Example `creating_a_training_module/requirements.txt`:

```text
jupyter-book==1.0.4
myst-parser==3.0.1
myst-nb==1.3.0
mdit-py-plugins==0.4.2
numpy
matplotlib
mplhep
awkward
uproot
```

Commit `creating_a_training_module/requirements.txt` (and optionally `requirements.lock`) and show learners how to activate the venv and install dependencies.

## Build, Check, and Preview
```bash
jupyter-book clean .
jupyter-book build -W --keep-going .   # fail on warnings
python -m http.server -d _build/html 8000
```
Open http://localhost:8000 to preview.


