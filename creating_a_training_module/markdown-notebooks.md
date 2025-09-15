# Jupyter Book MyST Markdown Notebooks

Jupyter Book also lets you write text-based notebooks using MyST Markdown.

## Create a notebook with MyST Markdown

MyST Markdown notebooks are defined by two things:

1. YAML metadata that is needed to understand if / how it should convert text files to notebooks (including information about the kernel needed).
   See the YAML at the top of this page for example.
2. The presence of `{code-cell}` directives, which will be executed with your book.

That's all!

An example of the YAML metadata needed is:

```yaml
---
jupytext:
  cell_metadata_filter: -all
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.16.4
kernelspec:
  display_name: Python 3 (ipykernel)
  language: python
  name: python3
--- 
```

See [the Notebooks with MyST Markdown documentation](https://jupyterbook.org/file-types/myst-notebooks.html) for more detailed instructions.


## An example cell

With MyST Markdown, you can define code cells with a directive like so:

````md
```{code-cell}
print(2 + 2)
```
````

When your book is built, the contents of any `{code-cell}` blocks will be
executed with your Jupyter kernel, and their outputs will be displayed
in-line with the rest of your content.

Let's download the [penguins.csv](https://raw.githubusercontent.com/hsf-training/deep-learning-intro-for-hep/refs/heads/main/deep-learning-intro-for-hep/data/penguins.csv) dataset from the Deep Learning from HEP course:

```bash
mkdir data && cd data
wget https://raw.githubusercontent.com/hsf-training/deep-learning-intro-for-hep/refs/heads/main/deep-learning-intro-for-hep/data/penguins.csv 
```

Install `pandas` in the environment you are using to build your book, and then run the following code cell to load the dataset:

````
```{code-cell} ipython3
import pandas as pd
penguins_df = pd.read_csv("data/penguins.csv")
penguins_df
```
````

The argument after `{code-cell}` (`ipython3`) is used for readability purposes.

Compile the book, and you should see the output of the code cell above.

## Visualizing data distributions

Let's use Matplotlib to visualize a regression problem with this dataset.

````md
For our regression problem, let's ask, "Given a flipper length (mm), what is the penguin's most likely body mass (g)?"

```{code-cell} ipython3
regression_features, regression_targets = penguins_df.dropna()[["flipper_length_mm", "body_mass_g"]].values.T
```

```{code-cell} ipython3
import matplotlib.pyplot as plt

fig, ax = plt.subplots()

def plot_regression_problem(ax, xlow=170, xhigh=235, ylow=2400, yhigh=6500):
    ax.scatter(regression_features, regression_targets, marker=".")
    ax.set_xlim(xlow, xhigh)
    ax.set_ylim(ylow, yhigh)
    ax.set_xlabel("flipper length (mm)")
    ax.set_ylabel("body mass (g)")

plot_regression_problem(ax)

plt.show()
```
````

## Working with packages 

You can install any packages you need in the environment you are using to build your book. It is a good practice to 
use `requirements.txt` files to manage your dependencies.

Let's add sklearn to our environment:

```bash
jupyter-book
matplotlib
numpy
pandas
scikit-learn
```

and install the packages as
```bash
pip install -r requirements.txt
```

Now we can use sklearn to build a regression model for our problem:

````md
Let's use Scikit-Learn's `LinearRegression`

```{code-cell} ipython3
from sklearn.linear_model import LinearRegression
import numpy as np
```

```{code-cell} ipython3
best_fit = LinearRegression().fit(regression_features[:, np.newaxis], regression_targets)
```

```{code-cell} ipython3
fig, ax = plt.subplots()

def plot_regression_solution(ax, model, xlow=170, xhigh=235):
    model_x = np.linspace(xlow, xhigh, 1000)
    model_y = model(model_x)
    ax.plot(model_x, model_y, color="tab:orange")

plot_regression_solution(ax, lambda x: best_fit.predict(x[:, np.newaxis]))
plot_regression_problem(ax)

plt.show()
```

```{code-cell} ipython3
print("slope:", best_fit.coef_[0])
print("intercept:", best_fit.intercept_)
```
````


## Quickly add YAML metadata for MyST Notebooks

If you have a markdown file and you'd like to quickly add YAML metadata to it, 
so that Jupyter Book will treat it as a MyST Markdown Notebook, run the following command:

```
jupyter-book myst init path/to/markdownfile.md
```

