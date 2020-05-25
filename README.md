# Data Science Template for Python

Inspired by and adapted from [Cookiecutter Data Science](https://drivendata.github.io/cookiecutter-data-science/).

## Usage

#### Installing `cookiecutter`

Install [cookiecutter](https://github.com/cookiecutter/cookiecutter) if it isn't
already.

```shell script
pip install --user cookiecutter
```

or

```shell script
conda config --add channels conda-forge
conda install cookiecutter
```

#### Creating the template

From a parent directory, run

```shell script
cookiecutter gh:ensley/ccdspy
```

Follow the prompts to do the initial setup of your project. Hit enter to accept
the defaults in [brackets].

```
project_name [project]:     # <- Name of the newly created folder and base Python package
author [Your name (or your company/organization/team]: 
description [A short description of the project.]: 
Select open_source_license:
1 - MIT
2 - BSD-3-Clause
3 - No license file
Choose from 1, 2, 3 [1]: 
```

#### Setting up the template

The first thing you'll want to do is create a fresh virtual environment with 
[venv](https://docs.python.org/3/library/venv.html), [pipenv](https://pypi.org/project/pipenv/),
or whatever you prefer. Then install the requirements in `requirements.txt`.

```shell script
cd <project>           <- replace <project> with the project name you specified
python3 -m venv ./.venv
source .venv/bin/activate
pip install -r requirements.txt
```

Now that [Sphinx](https://www.sphinx-doc.org/en/master/) has been installed, we
can create documentation stubs by running

```shell script
make docs
```

This has done two things:

1. There is Sphinx documentation already set up in `docsrc/`, which is pointing
to the package in `<project>`. First, `make docs` simply runs the Sphinx
`make html` directive from inside `docsrc/`.
2. Next, the built HTML in `docsrc/_build/html` is copied to the `docs/` folder.
This allows the documentation to easily be served as a static website by
pointing Github Pages to the "`docs/` folder in the `master` branch".

## Handling Jupyter notebooks

Using version control for Jupyter notebooks is awkward and generally
considered to be not a good idea; [see here](https://drivendata.github.io/cookiecutter-data-science/#notebooks-are-for-exploration-and-communication)
for a discussion of the drawbacks. In addition to those discussed, working on
projects in a corporate setting adds another pitfall: notebook output may
contain PII that we cannot under any circumstances allow to be checked into
version control.

As a result, we must handle notebooks with care. [Jupytext](https://jupytext.readthedocs.io/en/latest/)
is a good tool for this task, and it is included in this template's
`requirements.txt`. You may use it however you see fit, but this template
suggests storing `.ipynb` files in the `notebooks` folder and corresponding
`.py` files in the `notebook-scripts` folder. You may then, for example, sync
notebooks and scripts with `jupytext` and add `notebooks/` and/or `*.ipynb` to
the root `.gitignore` file so that only the scripts in `notebook-scripts/` are
committed.

An alternative option is to use a [Git hook](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks)
to run `jupytext` automatically when a commit is attempted. The `.githooks/`
folder in this template contains such a hook. Before every commit, the script
is triggered. It converts all notebooks in the Git index to scripts placed in
`notebook-scripts/`, and then removes `.ipynb` files from Git so that only the
scripts are actually committed. This way you don't have to think about
`jupytext` at all. If you want more control, however, using `jupytext` directly
is also a good approach.

To set up the Git hook, first initialize a Git repository if you haven't done
so already.

```shell script
git init
```

Then run:

```shell script
make githooks
```

This makes all files in `.githooks/` executable and configures Git to use that
folder for its hooks with `git config core.hooksPath .githooks`.

## Project Organization

When the template is created, the directory structure will look like this.


    ├── LICENSE
    ├── Makefile           <- Makefile with commands like `make data` or `make train`
    ├── README.md          <- The top-level README for developers using this project.
    ├── data
    │   ├── external       <- Data from third party sources.
    │   ├── interim        <- Intermediate data that has been transformed.
    │   ├── processed      <- The final, canonical data sets for modeling.
    │   └── raw            <- The original, immutable data dump.
    │
    ├── docs               <- Built HTML Sphinx docs can be copied here and served on Github Pages
    │
    ├── docsrc             <- A default Sphinx project; see sphinx-doc.org for details
    │
    ├── models             <- Trained and serialized models, model predictions, or model summaries
    │
    ├── notebooks          <- Jupyter notebooks. Naming convention is a number (for ordering),
    │                         the creator's initials, and a short `-` delimited description, e.g.
    │                         `1.0-jqp-initial-data-exploration`.
    │
    ├── notebook-scripts   <- Plain .py scripts generated from Jupyter notebooks
    │
    ├── references         <- Data dictionaries, manuals, and all other explanatory materials.
    │
    ├── reports            <- Generated analysis as HTML, PDF, LaTeX, etc.
    │   └── figures        <- Generated graphics and figures to be used in reporting
    │
    ├── requirements.txt   <- The requirements file for reproducing the analysis environment, e.g.
    │                         generated with `pip freeze > requirements.txt`
    │
    ├── setup.py           <- makes project pip installable (pip install -e .) so src can be imported
    ├── [project_name]     <- Source code for use in this project.
    │   ├── __init__.py    <- Makes this a Python package
    │   │
    │   ├── data           <- Scripts to download or generate data
    │   │   └── make_dataset.py
    │   │
    │   ├── features       <- Scripts to turn raw data into features for modeling
    │   │   └── build_features.py
    │   │
    │   ├── models         <- Scripts to train models and then use trained models to make
    │   │   │                 predictions
    │   │   ├── predict_model.py
    │   │   └── train_model.py
    │   │
    │   └── visualization  <- Scripts to create exploratory and results oriented visualizations
    │       └── visualize.py