---
layout: post
comments: true
title: "How to do your Machine Learning Assignments in 10 minutes"
author: martibosch
date: 2016-08-27
category: blog
tags: machine-learning python anaconda jupyter
---

Last semester, while I was taking the course of _Machine Learning_, I was given the following assignment:

> Comparison of the empirical error of the Perceptron, Logistic Regression, AdaBoost and SVM algorithms on the [UCI Datasets](https://archive.ics.uci.edu/ml/datasets.html) of "Breast Cancer Wisconsin (Diagnostic)", "Ionosphere", "Letter Recognition" and "Spambase".
>
> Instructions: Shuffle the datasets, and use a 70% of the data for training and 30% for testing. Repeat this process 20 times and list the average percentual empirical errors for both the training and testing stages.

The professor gave us a [link](http://ama.liglab.fr/~amini/DataCode.html) to his compilable code to train and test with the _Perceptron_, _Logistic Regression_ and _AdaBoost_ algorithms and another [link](http://svmlight.joachims.org/) for an executable _SVM_ implementation. The idea was then to craft some _bash_ scripts that did the dirty work for us.

I found that such assignment was rather more suitable for a _bash_ tutorial than for a Machine Learning course. So I basically borrowed the code from some nice friends (thank you guys), executed in my computer, and write a report with the results (which would be different than my friends' because of the dataset _shuffling_).

It was not until my Master Thesis internship at [STEEP](https://team.inria.fr/steep/) that I discovered [this](https://www.udemy.com/learning-python-for-data-analysis-and-visualization/) course, which introduced me to a working methodology that boosted greatly my productivity. Then I realized that with that methodology I could have done my assignment in 10 minutes (and enjoyed it more), which motivated me to write this post.

## Preparing our Working Environment

### First Step: Download Anaconda

**Anaconda** is an _open source_ **Python** distribution that comes with most of the _data science_ packages that you will need. Go to **Anaconda**'s [download page](https://www.continuum.io/downloads) and follow the instructions to install it in your system. You will need around 600MB of free disk space and it will take some time (that is not part of the promised 10 minutes, sorry) to install. For **UNIX** users, here are some _important notes_ (see [this](http://conda.pydata.org/docs/installation.html) for more information about the installation):

- Do NOT install as _super user_
- If installing in **Linux**: select `yes` when asked to prepend the install location to your `PATH` in your **bash** config (i.e. `~/.bashrc`)
- To have tab autocompletion in the terminal, do `echo 'eval "$(register-python-argcomplete conda)"' >> ~/.bashrc` (or change `~/.bashrc` for your bash config file)

### Create a Virtual Environment

Once you have successfully installed **Anaconda**, you can open a terminal and use the command `conda`. First we will create a _virtual environment_, which is just a bunch of **Python** packages (with a specified version each) wrapped up in a way that:

1. in your computer you can work on projects that use different versions of **Python** packages, _and_
2. you can automatically replicate such environment so your project will work the exact same way in another computer, regardless of its system packages.

So now that you know about **Python** _virtual environments_, let's create one named `data_science` as in: `conda create -n data_science anaconda`. This environment will come with the `anaconda` packages, which include already everything you might need for your assignments.
To work on the `data_science` environment do `source activate data_science` (and use `deactivate data_science` to exit it).

### Working with Jupyter Notebooks

Now that we already have a great stack of libraries that will help us on our _data science_ duties, it is time to put it into action in a _very interactive way_. The following tool is what has boosted the most my productivity as a _data scientist_: [**Jupyter Notebooks**](http://jupyter.org/). It is sort of a mix between an _interactive shell_ and a _code editor_, which allows to craft little pieces of code and execute, modify and export them. If you find that **Python** _shell_ allows very little _code editing_ and that writing `.py` _scripts_ and execute them is not too _interactive_, **Jupyter Notebooks** is just what you were looking for.

The **Jupyter Notebooks** come as part of the `anaconda` package set, and works as a web application. You might use it directly on the browser, or in **Emacs** (this is what I do, and works like a charm. See [this package](https://github.com/millejoh/emacs-ipython-notebook) to learn how), or **Vim** (although I have never used it).

To use it, just launch a terminal, activate your `data_science` _virtual environment_, set your desired _working directory_ and then execute `jupyter notebook`. With the default configuration, it will run at http://localhost:8888/ and automatically open a browser tab with that address (if you use **Emacs** or **Vim** you might want to change the default settings so it does not open the browser). Now create a new notebook for your assignment (in the browser you just go to `New > Notebook Python 3`), and let the game begin!

## Hands on the Data

### Loading the Datasets

To manipulate the data, we will use the [**pandas**](http://pandas.pydata.org/) package, which comes as part of the `anaconda` package distribution that we used in our environment.

So assuming that we are working in our _virtual environment_, we are going to import **pandas** and use its `read_csv` method that accepts URIs as parameter. We will do that as follows:

```python
import pandas as pd

base_uri = "https://archive.ics.uci.edu/ml/machine-learning-databases/"

uris = [
    base_uri + dataset_uri for dataset_uri in [
        "breast-cancer-wisconsin/wdbc.data",
        "ionosphere/ionosphere.data",
        "letter-recognition/letter-recognition.data",
        "spambase/spambase.data"
    ]
]

bc, io, lr, sp = [pd.read_csv(uri, header=None) for uri in uris]
```

If you actually check the input files from the URIs, you will see that the first line of the file is already an instance, which justifies the `header=None` passed as _keyword argument_.

Now we have four _DataFrame_ objects that correspond to each of the UCI Datasets.

### Understanding the Data

This is most likely the most boring part: it involves reading! Because well, we _would_ need to know what each _variable_ represents. But as applied mathematicians we just see a _supervised classification_ problem, among which we just need to spot the **_response_** and the _independent features_ or **_attributes_**. This time I will do it for you and summarize it into a nice table (but next time, you are going to have to browse the description of each of the dataset to spot the **_response_** and the **_attributes_**):

|                Dataset | #Instances | #Cols | Response                                                                     | Attributes                |
| ---------------------: | :--------: | :---: | ---------------------------------------------------------------------------- | ------------------------- |
|      **Breast Cancer** |    569     |  32   | _2nd col_ as in _'M'=malignant, 'B'=benign_                                  | \* _30 cols, 3rd to 32nd_ |
|         **Ionosphere** |    351     |  35   | _35th (last) col_ as in _'g'=good, 'b'=bad_                                  | _cols 1st to 34th_        |
| **Letter Recognition** |   20000    |  17   | _1st col_, _multi-classed_ with _26 values_ representing letters form A to Z | _cols 1st to 16th_        |
|           **Spambase** |    4601    |  58   | _58th (last) col_ as in _'0'=spam, '1'=not spam_                             | _cols 1st to 57th_        |

\*The _1st column_ of **Breast Cancer** represents the _patient id_, which should not be considered as it is not a determinant fact on the classification (again, this shows why we need to read the dataset descriptions)

**Very important note**: none of the datasets contained **_missing values_**, so there is no need for any data preprocessing in this sense. A deeper read of the datasets' attribute descriptions should tell us whether we need other data preprocessing such as _standardization_. But this is outside the scope of my post. I am just describing a generic dataset-independint working methodology. I will just mention that in the `anaconda` package distribution includes several tools to address the data preprocessing (i.e. http://scikit-learn.org/stable/modules/preprocessing.html)

So now we are ready to separate the **_response_** and the **_attributes_**. For that purpose, we will use the `iloc` method of the `DataFrame` **class**. For two-dimensional `DataFrame` instances, the first component selects among the _row axis_, and the second one among the _column axis_. So taking into account which columns we extract, with the following lines of code we will get the attributes as `X_foo` and the response as `y_foo`:

```python
X_bc, y_bc = bc.iloc[:, 2:], bc.iloc[:, 1]
X_io, y_io = io.iloc[:, :io.shape[1]-1], io.iloc[:, -1]
X_lr, y_lr = lr.iloc[:, 1:], lr.iloc[:, 0]
X_sp, y_sp = sp.iloc[:, :sp.shape[1]-1], sp.iloc[:, -1]

# Any preprocessing (normalization, missing values...) should be done here.

datasets = {
    label: (X, y) for label, X, y in [
        ('breast_cancer', X_bc, y_bc), ('ionosphere', X_io, y_io),
        ('letter_recognition', X_lr, y_lr), ('spambase', X_sp, y_sp)
    ]
}
```

The `shape` method returns a _tuple_ representing the dimensions of its `DataFrame` instance, so `foo.shape[1]` will return the number of _columns_ of `foo`. On the other hand, the `iloc` method automatically infers the type to each instance, so in our case the `y_foo` objects will be `Series` (one-dimensional n-arrays) whereas the attributes will still be `DataFrames` (two-dimensional n-arrays).

After the _individualized_ separation of each dataset's **_response_** and **_attributes_** (and potential data preprocessing in many real-life cases), we will pack all of the datasets into a labelled _dict_ `datasets`. Using a _dict_ is not necessary, we could have just created separate _variables_ for each _DataFrame_, however the _dict_ is easily iterable, which will be very useful to _DRY out_ our further code.

### Preparing the Training and Test Datasets

The assignment instructions state that we shall _shuffle_ the data, and then use a 70% of the instances for _training_ and the remaining 30% for _testing_.

Let me introduce you to the module that we are going to use for the _learning algorithms_: [**scikit-learn**](http://scikit-learn.org/stable/). The module comes as well as part of the `anaconda` package distribution, and has the `train_test_split` method that will perform all the tasks described in the paragraph above in just one line of code (for a given dataset's **_attributes_** `X` and **_response_** `y`):

```python
from sklearn.cross_validation import train_test_split

X_train, X_test, y_train, y_test = train_test_split(X, y, train_size=.7)
```

where the `train_size=.7` indicates that a 70% of the dataset shall be used as _training_ (and implicitly a 30% for _testing_).

## Let There Be Learning

After splitting our dataset, we are ready to _learn_ a _model_ out of the _training_ set, and test how well the model does on our _testing_ set. With **scikit-learn**, this could not be simplier:

1. We first import the _learning algorithm_ **class** (i.e. `Perceptron`) and _instanciate_ it
2. Learn the _algorithm_'s _model_ out of the _training_ set by calling the _instance_'s method `.fit(X_train, y_train)`
3. Determine the _model_'s accuracy over the _testing_ set by calling the _instance_'s method `.score(X_test, y_test)`

So with `Perceptron` as example, this code would look as follows:

```python
from sklearn.linear_model import Perceptron

model = Perceptron()
model.fit(X_train, y_train)
model.score(X_test, y_test)
```

## Putting it all Together

Let's go back to the assignment description: we want to compare the _empirical error_ of four different _learning algorithms_ in four different datasets. So first we need to import the _learning algorithms_, which are all four available in **scikit-learn**, and put them into some _iterable_ (I will use a _dict_ so I have associated labels at each iteration):

```python
from sklearn.linear_model import LogisticRegression, Perceptron
from sklearn.ensemble import AdaBoostClassifier
from sklearn.svm import SVC

algorithms = {
    'perceptron': Perceptron,
    'logistic_regression': LogisticRegression,
    'adaboost': AdaBoostClassifier,
    'svm': SVC
}
```

Now we are ready to wrap it all in three nested **for** _loops_. Before the _loop_, a `DataFrame` called `scores_df` will be created in order to store the _score_ (the complementary of the _empirical error_), with the _learning algorithms_ as _columns_ and the datasets as _rows_. Then the three **for** _loops_ (in increasing depth order) are the following:

- The outermost **for** _loop_ will iterate over the `algorithms` **dict**, _instantiating_ each _learning algorithm_ **class** as the `model` _variable_ and a **dict** `algorithm_scores` to store the _scores_ corresponding to the _loop_'s _algorithm_.

  - An inner **for** _loop_ is needed to iterate over the different datasets (for the same _algorithm_). To determine the dataset's _average score_, at this level the initialization of the _variable_ `dataset_score = 0` is done.

    - The innermost **for** _loop_ deals with the `n = 20` _trials_ as follows:
      1. First it _shuffles_ and _splits_ with `train_test_split`
      2. Then _learns_ the `model` on the _training_ dataset with `.fit`
      3. At the end it _adds_ the `i`th _trial_'s _score_ determined with `score` .over the _testing_ dataset

  - After performing all the `n` trials over the dataset, the _average_ _score_ is stored in the `algorithm_scores` under the dataset's label

- Finally, in order to add the results to the table `scores_df`, we must create a **pandas** `Series` out of `algorithm_scores` representing the _column_, which we can then add to the table.

```python
from sklearn.cross_validation import train_test_split

scores_df = pd.DataFrame(columns=algorithms.keys())
n = 20

for algorithm_label, algorithm_class in algorithms.items():
    model = algorithm_class()
    algorithm_scores = {}
    for dataset_label, (X, y) in datasets.items():
        dataset_score = 0
        for i in range(n):
            X_train, X_test, y_train, y_test = train_test_split(
                X, y, train_size=.7)
            model.fit(X_train, y_train)
            dataset_score += model.score(X_test, y_test)
        algorithm_scores[dataset_label] = dataset_score / n
    scores_df[algorithm_label] = pd.Series(algorithm_scores)
```

And now we can print the table with the results:

```python
scores_df
```

                        perceptron  adaboost  logistic_regression       svm
    breast_cancer         0.838012  0.960234             0.949415  0.632456
    ionosphere            0.811792  0.918396             0.857075  0.925472
    letter_recognition    0.459800  0.265075             0.717717  0.970200
    spambase              0.617379  0.937762             0.928639  0.821832

## Writing your Report

I am actually one of those _math_/_engineering_ students that actually enjoys writing nice reports with **LaTeX**. Nevertheless, it takes a lot of time, and in an _engineering_ school time it is a very limited resource. But **Jupyter** comes with an awesome feature that allows you to export the `.ipynb` notebooks to several formats. It is as easy as going to a terminal with the _virtual environment_ activated, and execute:

```bash
jupyter nbconvert --to latex assignment.ipynb
```

assuming that `assingment.ipynb` is your notebook (or _path_ to your notebook). The command will create an `assignment.tex` file in the _shell_'s _working directory_, which you might edit until you have your final report.

If you are very lazy, or do not enjoy writing reports at all, you might just add some explanatory _comments_ to your code, and execute:

```bash
jupyter nbconvert --to pdf assignment.ipynb
```

which will output the`assignment.pdf` that you might already use as final report.

## Final Notes

First of all, you might download the `assignment.ipynb` [here]({{ site.baseurl }}/assets/notebooks/machine_learning_assignment.ipynb), or [view it directly on the browser](https://github.com/martibosch/martibosch.github.io/blob/master/assets/notebooks/machine_learning_assignment.ipynb).

The main motivation of this post was the thought that I had of "if only I had known this while I was taking the course...", but working with **Anaconda** and **Jupyter** will certainly be very useful for any _data analysis_ task that I might do in the future. The "10 minutes" might be a bit exagerated, but I bet there is no faster way. If you want to learn more, I totally recommend you to check [Jose Portilla's course on **udemy**](https://www.udemy.com/learning-python-for-data-analysis-and-visualization/).

Another thing that I recommend you is to use **Emacs**. The learning curve is for sure steep, but it is worth it. Now I use it for coding in any language, web design, **LaTeX**, Markdown... This [slide presentation](http://chillaranand.github.io/emacs-py-ide/) should give you a nice introduction. Start before you are too old!
