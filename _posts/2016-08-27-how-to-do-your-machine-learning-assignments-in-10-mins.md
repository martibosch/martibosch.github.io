---
layout:   post
comments: true
title:    "How to do your Machine Learning Assignments in 10 minutes"
author:   martibosch
date:     2016-08-27
category: blog
tags: machine-learning python anaconda jupyter
---
Last semester, while I was taking the course of *Machine Learning*, I was given the following assignment:

> Comparison of the empirical error of the Perceptron, Logistic Regression, AdaBoost and SVM algorithms on the [UCI Datasets](https://archive.ics.uci.edu/ml/datasets.html) of "Breast Cancer Wisconsin (Diagnostic)", "Ionosphere", "Letter Recognition" and "Spambase".
>
> Instructions: Shuffle the datasets, and use a 70% of the data for training and 30% for testing. Repeat this process 20 times and list the average percentual empirical errors for both the training and testing stages.

The professor gave us a [link](http://ama.liglab.fr/~amini/DataCode.html) to his compilable code to train and test with the *Perceptron*, *Logistic Regression* and *AdaBoost* algorithms and another [link](http://svmlight.joachims.org/) for an executable *SVM* implementation. The idea was then to craft some *bash* scripts that did the dirty work for us.

I found that such assignment was rather more suitable for a *bash* tutorial than for a Machine Learning course. So I basically borrowed the code from some nice friends (thank you guys), executed in my computer, and write a report with the results (which would be different than my friends' because of the dataset *shuffling*).

It was not until my Master Thesis internship at [STEEP](https://team.inria.fr/steep/) that I discovered [this](https://www.udemy.com/learning-python-for-data-analysis-and-visualization/) course, which introduced me to a working methodology that boosted greatly my productivity. Then I realized that with that methodology I could have done my assignment in 10 minutes (and enjoyed it more), which motivated me to write this post.

## Preparing our Working Environment

### First Step: Download Anaconda

**Anaconda** is an *open source* **Python** distribution that comes with most of the *data science* packages that you will need. Go to **Anaconda**'s [download page](https://www.continuum.io/downloads) and follow the instructions to install it in your system. You will need around 600MB of free disk space and it will take some time (that is not part of the promised 10 minutes, sorry) to install. For **UNIX** users, here are some *important notes* (see [this](http://conda.pydata.org/docs/installation.html) for more information about the installation):
    
* Do NOT install as *super user*
* If installing in **Linux**: select `yes` when asked to prepend the install location to your `PATH` in your **bash** config (i.e. `~/.bashrc`)
* To have tab autocompletion in the terminal, do `echo 'eval "$(register-python-argcomplete conda)"' >> ~/.bashrc` (or change `~/.bashrc` for your bash config file)

### Create a Virtual Environment

Once you have successfully installed **Anaconda**, you can open a terminal and use the command `conda`. First we will create a *virtual environment*, which is just a bunch of **Python** packages (with a specified version each) wrapped up in a way that:

1. in your computer you can work on projects that use different versions of **Python** packages, *and*
2. you can automatically replicate such environment so your project will work the exact same way in another computer, regardless of its system packages.

So now that you know about **Python** *virtual environments*, let's create one named `data_science` as in: `conda create -n data_science anaconda`. This environment will come with the `anaconda` packages, which include already everything you might need for your assignments.
To work on the `data_science` environment do `source activate data_science` (and use `deactivate data_science` to exit it).

### Working with Jupyter Notebooks

Now that we already have a great stack of libraries that will help us on our *data science* duties, it is time to put it into action in a *very interactive way*. The following tool is what has boosted the most my productivity as a *data scientist*: [**Jupyter Notebooks**](http://jupyter.org/). It is sort of a mix between an *interactive shell* and a *code editor*, which allows to craft little pieces of code and execute, modify and export them. If you find that **Python** *shell* allows very little *code editing* and that writing `.py` *scripts* and execute them is not too *interactive*, **Jupyter Notebooks** is just what you were looking for.

The **Jupyter Notebooks** come as part of the `anaconda` package set, and works as a web application. You might use it directly on the browser, or in **Emacs** (this is what I do, and works like a charm. See [this package](https://github.com/millejoh/emacs-ipython-notebook) to learn how), or **Vim** (although I have never used it).

To use it, just launch a terminal, activate your `data_science` *virtual environment*, set your desired *working directory* and then execute `jupyter notebook`. With the default configuration, it will run at http://localhost:8888/ and automatically open a browser tab with that address (if you use **Emacs** or **Vim** you might want to change the default settings so it does not open the browser). Now create a new notebook for your assignment (in the browser you just go to `New > Notebook Python 3`), and let the game begin!

## Hands on the Data

### Loading the Datasets

To manipulate the data, we will use the [**pandas**](http://pandas.pydata.org/) package, which comes as part of the `anaconda` package distribution that we used in our environment.

So assuming that we are working in our *virtual environment*, we are going to import **pandas** and use its `read_csv` method that accepts URIs as parameter. We will do that as follows:

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

If you actually check the input files from the URIs, you will see that the first line of the file is already an instance, which justifies the `header=None` passed as *keyword argument*.

Now we have four *DataFrame* objects that correspond to each of the UCI Datasets. 

### Understanding the Data

This is most likely the most boring part: it involves reading! Because well, we *would* need to know what each *variable* represents. But as applied mathematicians we just see a *supervised classification* problem, among which we just need to spot the **_response_** and the *independent features* or **_attributes_**. This time I will do it for you and summarize it into a nice table (but next time, you are going to have to browse the description of each of the dataset to spot the **_response_** and the **_attributes_**):

| Dataset | #Instances | #Cols | Response | Attributes |
| -----: | :-----: | :-----: | ----- | ----- |
| **Breast Cancer** | 569 | 32 | *2nd col* as in *'M'=malignant, 'B'=benign* | \* *30 cols, 3rd to 32nd* |
| **Ionosphere** | 351 | 35 | *35th (last) col* as in *'g'=good, 'b'=bad* | *cols 1st to 34th* |
| **Letter Recognition** | 20000 | 17 | *1st col*, *multi-classed* with *26 values* representing letters form A to Z  | *cols 1st to 16th* |
| **Spambase** | 4601 | 58 | *58th (last) col* as in *'0'=spam, '1'=not spam* | *cols 1st to 57th* |

\*The *1st column* of **Breast Cancer** represents the *patient id*, which should not be considered as it is not a determinant fact on the classification (again, this shows why we need to read the dataset descriptions)

**Very important note**: none of the datasets contained **_missing values_**, so there is no need for any data preprocessing in this sense. A deeper read of the datasets' attribute descriptions should tell us whether we need other data preprocessing such as *standardization*. But this is outside the scope of my post. I am just describing a generic dataset-independint working methodology. I will just mention that in the `anaconda` package distribution includes several tools to address the data preprocessing (i.e. http://scikit-learn.org/stable/modules/preprocessing.html)

So now we are ready to separate the **_response_** and the **_attributes_**. For that purpose, we will use the `iloc` method of the `DataFrame` **class**. For two-dimensional `DataFrame` instances, the first component selects among the *row axis*, and the second one among the *column axis*. So taking into account which columns we extract, with the following lines of code we will get the attributes as `X_foo` and the response as `y_foo`:

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

The `shape` method returns a *tuple* representing the dimensions of its `DataFrame` instance, so `foo.shape[1]` will return the number of *columns* of `foo`. On the other hand, the `iloc` method automatically infers the type to each instance, so in our case the `y_foo` objects will be `Series` (one-dimensional n-arrays) whereas the attributes will still be `DataFrames` (two-dimensional n-arrays).

After the *individualized* separation of each dataset's **_response_** and **_attributes_** (and potential data preprocessing in many real-life cases), we will pack all of the datasets into a labelled *dict* `datasets`. Using a *dict* is not necessary, we could have just created separate *variables* for each *DataFrame*, however the *dict* is easily iterable, which will be very useful to *DRY out* our further code.
 

### Preparing the Training and Test Datasets

The assignment instructions state that we shall *shuffle* the data, and then use a 70% of the instances for *training* and the remaining 30% for *testing*.

Let me introduce you to the module that we are going to use for the *learning algorithms*: [**scikit-learn**](http://scikit-learn.org/stable/). The module comes as well as part of the `anaconda` package distribution, and has the `train_test_split` method that will perform all the tasks described in the paragraph above in just one line of code (for a given dataset's **_attributes_** `X` and **_response_** `y`):

```python
from sklearn.cross_validation import train_test_split

X_train, X_test, y_train, y_test = train_test_split(X, y, train_size=.7)
```

where the `train_size=.7` indicates that a 70% of the dataset shall be used as *training* (and implicitly a 30% for *testing*).


## Let There Be Learning

After splitting our dataset, we are ready to *learn* a *model* out of the *training* set, and test how well the model does on our *testing* set. With **scikit-learn**, this could not be simplier:

1. We first import the *learning algorithm* **class** (i.e. `Perceptron`) and *instanciate* it
2. Learn the *algorithm*'s *model* out of the *training* set by calling the *instance*'s method `.fit(X_train, y_train)`
3. Determine the *model*'s accuracy over the *testing* set by calling the *instance*'s method `.score(X_test, y_test)`

So with `Perceptron` as example, this code would look as follows:

```python
from sklearn.linear_model import Perceptron

model = Perceptron()
model.fit(X_train, y_train)
model.score(X_test, y_test)
```

## Putting it all Together

Let's go back to the assignment description: we want to compare the *empirical error* of four different *learning algorithms* in four different datasets. So first we need to import the *learning algorithms*, which are all four available in **scikit-learn**, and put them into some *iterable* (I will use a *dict* so I have associated labels at each iteration):

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

Now we are ready to wrap it all in three nested **for** *loops*. Before the *loop*, a `DataFrame` called `scores_df` will be created in order to store the *score* (the complementary of the *empirical error*), with the *learning algorithms* as *columns* and the datasets as *rows*. Then the three **for** *loops* (in increasing depth order) are the following:

* The outermost **for** *loop* will iterate over the `algorithms` **dict**, *instantiating* each *learning algorithm* **class** as the `model` *variable* and a **dict** `algorithm_scores` to store the *scores* corresponding to the *loop*'s *algorithm*.
  * An inner **for** *loop* is needed to iterate over the different datasets (for the same *algorithm*). To determine the dataset's *average score*, at this level the initialization of the *variable* `dataset_score = 0` is done.
    * The innermost **for** *loop* deals with the `n = 20` *trials* as follows:
      1. First it *shuffles* and *splits* with `train_test_split`
      2. Then *learns* the `model` on the *training* dataset with `.fit`
      3. At the end it *adds* the `i`th *trial*'s *score* determined with `score` .over the *testing* dataset

  * After performing all the `n` trials over the dataset, the *average* *score* is stored in the `algorithm_scores` under the dataset's label
* Finally, in order to add the results to the table `scores_df`, we must create a **pandas** `Series` out of `algorithm_scores` representing the *column*, which we can then add to the table.

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

I am actually one of those *math*/*engineering* students that actually enjoys writing nice reports with **LaTeX**. Nevertheless, it takes a lot of time, and in an *engineering* school time it is a very limited resource. But **Jupyter** comes with an awesome feature that allows you to export the `.ipynb` notebooks to several formats. It is as easy as going to a terminal with the *virtual environment* activated, and execute:

```bash
jupyter nbconvert --to latex assignment.ipynb
```

assuming that `assingment.ipynb` is your notebook (or *path* to your notebook). The command will create an `assignment.tex` file in the *shell*'s *working directory*, which you might edit until you have your final report.

If you are very lazy, or do not enjoy writing reports at all, you might just add some explanatory *comments* to your code, and execute:

```bash
jupyter nbconvert --to pdf assignment.ipynb
```

which will output the`assignment.pdf` that you might already use as final report.


## Final Notes

First of all, you might download the `assignment.ipynb` [here]({{ site.baseurl }}/assets/notebooks/machine_learning_assignment.ipynb), or [view it directly on the browser](https://github.com/martibosch/martibosch.github.io/blob/master/assets/notebooks/machine_learning_assignment.ipynb).

The main motivation of this post was the thought that I had of "if only I had known this while I was taking the course...", but working with **Anaconda** and **Jupyter** will certainly be very useful for any *data analysis* task that I might do in the future. The "10 minutes" might be a bit exagerated, but I bet there is no faster way. If you want to learn more, I totally recommend you to check [Jose Portilla's course on **udemy**](https://www.udemy.com/learning-python-for-data-analysis-and-visualization/).

Another thing that I recommend you is to use **Emacs**. The learning curve is for sure steep, but it is worth it. Now I use it for coding in any language, web design, **LaTeX**, Markdown... This [slide presentation](http://chillaranand.github.io/emacs-py-ide/) should give you a nice introduction. Start before you are too old!
