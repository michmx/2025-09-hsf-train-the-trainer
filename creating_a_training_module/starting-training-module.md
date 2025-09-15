# Creating Training Modules with Jupyter Book

This chapter shows trainers in the HEP community how to develop and publish a self‑contained training module 
using Jupyter Book and the cookiecutter template. 

## Installation

We require 

- Python 3.9+ with a virtual environment (venv or conda/mamba).
- `jupyter-book` 
- Optional but recommended: `cookiecutter` 

Start the environment

```bash
python -m venv venv 
source venv/bin/activate
```

Install packages

```bash
pip install jupyter-book
pip install cookiecutter 
```

## Scaffold with Cookiecutter

Use the official template to generate a new book repo (for a fresh module):

```bash
cookiecutter gh:executablebooks/cookiecutter-jupyter-book
# Answer prompts (project_name, author, etc.)
cd your_book_name
```

Key files: `_config.yml` (book metadata), `_toc.yml` (structure), content pages (`.md`, `.ipynb`).

Let's build the book to see the default content:

```bash
cd your_book_name
jupyter-book build .
```

Open `_build/html/index.html` in a browser to preview.

## Adding Content

Let's create a new chapter file `linear_regresion.md` in the book directory. 

```md
# Linear Regression with scikit-learn

This chapter teaches linear regression using `scikit-learn` with a dataset from 
[Deep Learning from HEP](https://hsf-training.github.io/deep-learning-intro-for-hep/).

Download the dataset from [here](https://github.com/hsf-training/deep-learning-intro-for-hep/blob/main/deep-learning-intro-for-hep/data/penguins.csv) 
and place it in a `data/` folder.

The dataset contains the basic measurements on 3 species of penguins!   

![A penguin](https://hsf-training.github.io/deep-learning-intro-for-hep/_images/culmen_depth.png)
```

Modify the `_toc.yml` to add your chapter:

```yaml
# Table of contents
# Learn more at https://jupyterbook.org/customize/toc.html

format: jb-book
root: intro
chapters:
- file: linear_regresion.md
```

## Content blocks

You can add objectives with an admonition:

````md
```{admonition} Objectives
- Have fun with penguins!
```
````

Warnings are also available:

````md
```{warning}
Penguins can be very cute.
```
````

Look at the [Jupyter Book documentation](https://jupyterbook.org/en/stable/content/content-blocks.html) for more styles.