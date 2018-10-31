---
layout:   post
comments: true
title:    "Interactive development enviornment for data-centric Python development"
date:     2018-10-01
category: blog
tags: Python
---

## Assessment of the flood regulation

Task description

* Preprocessing data
  * swisslandstats-geopy
* Implementation of the STREAM model

Source control, data, collaboration and reproducibility

We are going to cover a lot of ground with this post. For the sake of brevity, I will skip many specifics, however I shall try to leave the links that I consider appropriate in case the reader wants to go into more details. This post is about *breadth* rather than *depth*.

## Setting up an interactive development environment

Figure

Our development environment will have two kinds of git repositories. There will be one and only one of the first kind, which we will refer to as the **analysis repository**, and will mostly consist of Jupyter notebooks. On the other hand, some of the tasks that we will perform in this analysis might also be needed in other analysis cases that we (and maybe other researchers as well) perform in the future. In this example, we have two of such tasks, i.e., the processing of the land statistics data from the Swiss Federal Statistical office and the simulation of the hydrological runoff. It might thus be worth organizing the code that deals with such in separated packages that we (and again, maybe other researchers as well) can reuse later. These packages correspond to the second kind of git repository, which we will call **package repositories**. A given **analysis repository** might use several **package repositories**, but since package repositories are intended to perform generic tasks, they might be reused within other **analysis repositories**. I hope this gets clearer with the specific example that we will present below.

### Preparing the main environment

1. Create the conda environment (in this case, for Python 3.6)

```bash
# the environment's name will be `floodreg_vaud`
$ conda env create floodreg_vaud python=3.6
```

2. Setup DVC and a data remote. In this post, we will be using DigitalOcean's Spaces, but it could be with Amazon S3, Microsoft Azure, Google Cloud Storage and the like.

3. Enter the fresh environment

```bash
$ conda activate floodreg_vaud
```

4. Already within the environment, make it available as a `jupyter` kernel as in:

```bash
$ python -m ipykernel install --user --name floodreg_vaud --display-name \
    "Python (floodreg_vaud)"
```

### Creating the Python package repositories

Concerns:

* Installable via PyPi (setuptools), with versioning. See also conda-recipes
* Continuous integration (tests and coverage), managed by tox for Python 2.7 and 3.6
* Licensed  https://choosealicense.com/
* Code formatting (yapf)
* Documentation

Python Package directory structure https://python-packaging.readthedocs.io/en/latest/minimal.html

I strongly reccomend that if it is your first time creating a Python Package, you go for **Option 1**

#### Option 1: Create the repository in GitHub and manually set up setuptools and the Python Package structure

The main advantage of creating a Python package repo in GitHub is to use their `LICENSE` and `.gitignore` files.
It can also automatically create a `README.md` file.

![Creating the repo in Github](/assets/images/interactive_development/python_package_repo_github.png)

Clone the repo locally and go to its root directory, e.g.,

```bash
# or `https://github.com/martibosch/pystream.git`
$ git clone git@github.com:martibosch/pystream.git
$ cd pystream/
```

We are then going to create a `requirements.txt` file [to specify the package dependences for the package that we intend to develop](https://pip.readthedocs.io/en/latest/reference/pip_install/#requirements-file-format). Of course at this point we might not know that yet, but let's assume the following:

```
numpy >= 1.15
rasterio >= 1.0.0
```

Now we are ready to create a `setup.py` for setuptools, which will be of the following form:

```python
from setuptools import setup

__version__ = '0.0.1'

# Get the long description from the README file
with open('README.md', encoding='utf-8') as f:
    long_description = f.read()

# Get the dependencies from the requirements file
with open('requirements.txt', encoding='utf-8') as f:
    install_requires = [req.strip() for req in f.read().split('\n')]

setup(
    name='pystream',
    version=__version__,
    description='Python implementation of the STREAM hydrological rainfall-'
    'runoff model',
    long_description=long_description,
    long_description_content_type='text/markdown',
    url='https://github.com/martibosch/pystream',
    author='Martí Bosch',
    author_email='marti.bosch@epfl.ch',
    license='GPL-3.0',
    packages=['stream'],
    include_package_data=True,
    install_requires=install_requires,
    dependency_links=dependency_links,
)
```

This is a rather minimalistic example, and I believe that most of its details are self-explanatory. We are just going to comment on one little detail (you might check [the Python Packaging tutorial](https://python-packaging.readthedocs.io/en/latest/index.html) and [the Setuptools documentation](http://peak.telecommunity.com/DevCenter/setuptools) for more details). Note that in the call to `setup`, the keyword argument `name` refers to `pystream`, whereas `packages` refers to `stream`. This is because the former corresponds to the "Distribution Package" name, i.e., the name that users will use to pip-install your package, whereas the latter corresponds to the "Import Package" name, i.e., the name that users will use to import your package within Python code (see the [packaging glossary](packaging.python.org/glossary) for more details on the terminology). The two might coincide for many Python packages. When I am working with models that are agnostic to the programming language (e.g., the STREAM model of this example), I like the "Distribution Package" name to contain some reference to Python (e.g., "py") since it will also correspond to the name of the GitHub repository. In contraposition, I find that including a reference to Python within the "Import Package" name, which will be used within Python code, might be redundant. But this is just a matter of taste[^fn-multiple-import-pkgs]. 

[^fn-multiple-import-pkgs]: It is worth noting that, although rare, very large projects (e.g. [the Zope web application server](https://github.com/zopefoundation/Zope)) might consist of multiple "Import Packages" that ship within a single "Distribution Package".

Anyway, now we need to create a Python module for our package, which assuming that the current directory is the project's root, can be done as:

```bash
$ mkdir stream
$ touch stream/__init__.py  # creates an empty file
```

Note that the directory name corresponds to the "Import Package" name and not the "Distribution Package" name. The empty `__init__.py` file just tells Python that the folder containing it is a module, i.e., something that Python can import.

**TODO**: `.travis.yml`

**TODO**: `setup.cfg` file

That's it, now you might add, commit and push the changes to the GitHub repository

```bash
# optional: add `.travis.yml` and/or `setup.cfg`
$ git add requirements.txt setup.py stream/__init__.py
$ git commit -m "Set up setuptools"
$ git push origin master
```

#### Option 2: Create the repository from a Cookiecutter template

**TODO:** As you create different Python projects, you might find the former task a bit repetitive. Fortunately, [cookiecutter](https://cookiecutter.readthedocs.io/en/latest/index.html) lets you automate it by using project templates. There are [many available templates to create Python packages](https://github.com/audreyr/cookiecutter#python). Some of them are very minimalistic, whereas others like [cookiecutter-pylibrary](https://github.com/ionelmc/cookiecutter-pylibrary) are "all-inclusive" and allow you to automatically configure tools for testing, documentation and versioning. If you are not happy with any of them, you can always fork one and customize it, [like I did](https://github.com/martibosch/cookiecutter-pipproject). 

### Installing an editable package from a git repository with pip

Once the repo is up in GitHub, you can use pip to install it in ["editable mode"](https://pip.pypa.io/en/stable/reference/pip_install/#editable-installs) by using the `-e` option. Make sure that you are at the root of the **analysis repository**, and the virtual environment that we created above (i.e., `floodreg_vaud` for this example) is active. This can be done as follows (note that for `egg` we use the "Distribution Package" name `pystream` and not the "Import Package" name):

```bash
# or `git+https://github.com/martibosch/pystream.git#egg=pystream`
$ pip install -e git+git@github.com:martibosch/pystream.git#egg=pystream
```

The above command will create a new directory `src` at the **analysis repository** root, which will host all the editable installs that we perform via pip from that location. If we navigate to that directory, we will see that our `pystream` repository has been cloned there

```bash
$ cd src/pystream/
$ git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```

## Footnotes
