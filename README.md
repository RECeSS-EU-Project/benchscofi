![funding logo](https://raw.githubusercontent.com/RECeSS-EU-Project/RECeSS-EU-Project.github.io/main/assets/images/header%2BEU_rescale.jpg)

[![Python Version](https://img.shields.io/badge/python-3.8-pink)](https://badge.fury.io/py/benchscofi) [![PyPI version](https://img.shields.io/pypi/v/benchscofi.svg)](https://badge.fury.io/py/benchscofi) [![Zenodo version](https://zenodo.org/badge/DOI/10.5281/zenodo.8241505.svg)](https://doi.org/10.5281/zenodo.8241505) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) [![Build Status](https://github.com/recess-eu-project/benchscofi/actions/workflows/post-push-test.yml/badge.svg)](https://github.com/recess-eu-project/benchscofi/actions/workflows/post-push-test.yml) [![Codecov](https://codecov.io/github/recess-eu-project/benchscofi/coverage.svg?branch=master)](https://codecov.io/github/recess-eu-project/benchscofi?branch=master) [![JOSS](https://joss.theoj.org/papers/10.21105/joss.05973/status.svg)](https://doi.org/10.21105/joss.05973)

# BENCHmark for drug Screening with COllaborative FIltering (benchscofi) Python Package

This repository is a part of the EU-funded [RECeSS project](https://recess-eu-project.github.io) (#101102016), and hosts the implementations and / or wrappers to published implementations of collaborative filtering-based algorithms for easy benchmarking.

## Statement of need

As of 2022, current drug development pipelines last around 10 years, costing $2billion in average, while drug commercialization failure rates go up to 90%. These issues can be mitigated by drug repurposing, where chemical compounds are screened for new therapeutic indications in a systematic fashion. In prior works, this approach has been implemented through collaborative filtering. This semi-supervised learning framework leverages known drug-disease matchings in order to recommend new ones.

There is no standard pipeline to train, validate and compare collaborative filtering-based repurposing methods, which considerably limits the impact of this research field. In **benchscofi**, the estimated improvement over the state-of-the-art (implemented in the package) can be measured through adequate and quantitative metrics tailored to the problem of drug repurposing across a large set of publicly available drug repurposing datasets.

## Install the latest release

The fastest way to get access to all functionalities of *benchscofi* is to run the following command:

```bash
## Using the Docker image: will open a container
docker push recessproject/benchscofi:1.0.1
```
Documentation about *benchscofi* (and a manual installation) can be found at [this page](https://recess-eu-project.github.io/stanscofi/). The complete list of dependencies for *benchscofi* can be found at [requirements.txt](https://raw.githubusercontent.com/RECeSS-EU-Project/benchscofi/master/pip/requirements.txt) (pip).

## Licence

This repository is under an [OSI-approved](https://opensource.org/licenses/) [MIT license](https://raw.githubusercontent.com/RECeSS-EU-Project/benchscofi/master/LICENSE). 

## Citation

If you use *benchscofi* in academic research, please cite it as follows

```
@article{reda2024stanscofi,
  title={stanscofi and benchscofi: a new standard for drug repurposing by collaborative filtering},
  author={R{\'e}da, Cl{\'e}mence and Vie, Jill-J{\^e}nn and Wolkenhauer, Olaf},
  journal={Journal of Open Source Software},
  volume={9},
  number={93},
  pages={5973},
  year={2024}
}
```

## Community guidelines with respect to contributions, issue reporting, and support

You are more than welcome to add your own algorithm to the package!

### 1. Add a novel implementation / algorithm

Add a new Python file (extension .py) in ``src/benchscofi/`` named ``<model>`` (where ``model`` is the name of the algorithm), which contains a subclass of ``stanscofi.models.BasicModel`` **which has the same name as your Python file**. At least implement methods ``preprocessing``, ``model_fit``, ``model_predict_proba``, and a default set of parameters (which is used for testing purposes). Please have a look at the placeholder file ``Constant.py`` which implements a classification algorithm which labels all datapoints as positive. It is highly recommended to provide a proper documentation of your class, along with its methods. When pushing a new algorithm to *benchscofi*, it is automatically tested (see *tests/test_models.py* and *TemplateTest.py* which are run). In order to run this test locally, please run in the ``tests/`` folder:

```bash
python3 -m test_models <model> <dataset:default=Synthetic>
```

### 2. Rules for contributors

[Pull requests](https://github.com/RECeSS-EU-Project/benchscofi/pulls) and [issue flagging](https://github.com/RECeSS-EU-Project/benchscofi/issues) are welcome, and can be made through the GitHub interface. Support can be provided by reaching out to ``recess-project[at]proton.me``. However, please note that contributors and users must abide by the [Code of Conduct](https://github.com/RECeSS-EU-Project/benchscofi/blob/master/CODE%20OF%20CONDUCT.md).


# Benchmark AUC and NDCG@items values (default parameters, single random training/testing set split) [updated 08/11/23]

These values (rounded to the closest 3rd decimal place) can be reproduced using the following command in folder ``tests/``

```bash
python3 -m test_models <algorithm> <dataset:default=Synthetic> <batch_ratio:default=1>
```

:no_entry:'s represent failure to train or to predict. ``N/A``'s have not been tested yet. When present, percentage in parentheses is the considered value of batch_ratio (to avoid memory crash on some of the datasets).
[mem]: memory crash
[err]: error

  Algorithm  (global AUC)  | Synthetic*    | TRANSCRIPT    [a] | Gottlieb [b]  | Cdataset [c] | PREDICT    [d] | LRSSL [e] | 
-------------------------- | ------------- | ----------------- | ------------- | ------------ | -------------- | --------- |
PMF                     |  0.922        |  0.579            |  0.598        |  0.604       |  0.656         | 0.611     |
PulearnWrapper          |  1.000        |  :no_entry:       |  N/A          |  :no_entry:  |  :no_entry:    | :no_entry:|
ALSWR                   |  0.971        |  0.507            |  0.677        |  0.724       |  0.693         | 0.685     |
FastaiCollabWrapper     |  1.000        |  0.876            |  0.856        |  0.837       |  0.835         | 0.851     |
SimplePULearning        |  0.995        |  0.949 (0.4)      |:no_entry:[err]|:no_entry:[err]| 0.994 (4%)    | :no_entry:|
SimpleBinaryClassifier  |  0.876        |  :no_entry:[mem]  |  0.855        |  0.938 (40%) |  0.998 (1%)    | :no_entry:|
NIMCGCN                 |  0.907        |  0.854            |  0.843        |  0.841       |  0.914 (60%)   | 0.873     |
FFMWrapper              |  0.924        |  :no_entry:[mem]  |  1.000 (40%)  |  1.000 (20%) |:no_entry:[mem] | :no_entry:|
VariationalWrapper      |:no_entry:[err]|  :no_entry:[err]  |  0.851        |  0.851       |:no_entry:[err] | :no_entry:|
DRRS                    |:no_entry:[err]|  0.662            |  0.838        |  0.878       |:no_entry:[err] | 0.892     |
SCPMF                   |  0.853        |  0.680            |  0.548        |  0.538       |:no_entry:[err] | 0.708     |
BNNR                    |  1.000        |  0.922            |  0.949        |  0.959       |  0.990 (1%)    | 0.972     |
LRSSL                   |  0.127        |  0.581 (90%)      |  0.159        |  0.846       |  0.764 (1%)    | 0.665     |
MBiRW                   |  1.000        |  0.913            |  0.954        |  0.965       |:no_entry:[err] | 0.975     |
LibMFWrapper            |  1.000        |  0.919            |  0.892        |  0.912       |  0.923         | 0.873     |
LogisticMF              |  1.000        |  0.910            |  0.941        |  0.955       |  0.953         | 0.933     |
PSGCN                   |  0.767        |  :no_entry:[err]  |  0.802        |  0.888       |  :no_entry:    | 0.887     |
DDA_SKF                 |  0.779        |  0.453            |  0.544        |  0.264 (20%) |  0.591         | 0.542     |
HAN                     |  1.000        |  0.870            |  0.909        |  0.905       |  0.904         | 0.923     |
PUextraTrees  (``n_estimators=10``)    |  0.045 (50%)  |  0.325 (50%)      |  0.246 (20%) |:no_entry:[mem] | 0.309 (5%)|
XGBoost  (``n_estimators=100``)        |  0.500        |  0.500 (20%)      |  0.500       |  0.500         |  0.500 (1%)       |  0.500 (60%)       |

The NDCG score is computed across all diseases (global), at k=#items.

Algorithm (global NDCG@k)  | Synthetic@300*| TRANSCRIPT@613[a] |Gottlieb@593[b]|Cdataset@663[c]|PREDICT@1577[d]|LRSSL@763[e]| 
-------------------------- | ------------- | ----------------- | ------------- | ------------ | -------------- | --------- |
PMF                     |  0.070        |  0.019            |  0.015        |  0.011       |  0.005         | 0.007     |
PulearnWrapper          |  N/A          |  :no_entry:       |  N/A          |  :no_entry:  |  :no_entry:    | :no_entry:|
ALSWR                   |  0.000        |  0.177            |  0.236        |  0.406       |  0.193         | 0.424     |
FastaiCollabWrapper     |  1.000        |  0.035            |  0.012        |  0.003       |  0.001         | 0.000     |
SimplePULearning        |  1.000        |  0.059 (40%)      |:no_entry:[err]|:no_entry:[err]| 0.025 (4%)    |:no_entry:[err]|
SimpleBinaryClassifier  |  0.000        |  :no_entry:[mem]  |  0.002        |  0.005 (40%) |  0.070 (1%)    |:no_entry:[err]|
NIMCGCN                 |  0.568        |  0.022            |  0.006        |  0.005       |  0.007 (60%)   | 0.014     |
FFMWrapper              |  1.000        |  :no_entry:[mem]  |  1.000 (40%)  |  1.000 (20%) |:no_entry:[mem] | :no_entry:|
VariationalWrapper      |:no_entry:[err]|  :no_entry:[err]  |  0.011        |  0.010       |:no_entry:[err] | :no_entry:|
DRRS                   |:no_entry:[err]|  0.484            |  0.301        |  0.426       |:no_entry:[err] | 0.182     |
SCPMF                  |  0.528        |  0.102            |  0.025        |  0.011       |:no_entry:[err] | 0.008     |
BNNR                   |  1.000        |  0.466            |  0.417        |  0.572       |  0.217 (1%)    | 0.508     |
LRSSL                  |  0.206        |  0.032 (90%)      |  0.009        |  0.004       |  0.103 (1%)    | 0.012     |
MBiRW                  |  1.000        |  0.085            |  0.267        |  0.352       |:no_entry:[err] | 0.457     |
LibMFWrapper           |  1.000        |  0.419            |  0.431        |  0.605       |  0.502         | 0.430     |
LogisticMF             |  1.000        |  0.323            |  0.106        |  0.101       |  0.076         | 0.078     |
PSGCN                  |  0.969        |  :no_entry:[err]  |  0.074        |  0.052       |:no_entry:[err] | 0.110     |
DDA_SKF                |  1.000        |  0.039            |  0.069        |  0.078 (20%) |  0.065         | 0.069     |
HAN                    |  1.000        |  0.075            |  0.007        |  0.000       |  0.001         | 0.002     |
PUextraTrees  (``n_estimators=10``)    |  0.000 (50%)  |  0.198 (50%)      |  0.162 (20%) |:no_entry:[mem] | 0.235 (5%)|
XGBoost  (``n_estimators=100``)        |  0.061        |  0.000 (20%)      |  0.002       |  0.000         |  0.000 (1%)       |  0.000 (60%)      |

:no_entry: Note that results from ``LibMFWrapper'' are not reproducible, and the resulting metrics might slightly vary across iterations.

:no_entry: XGBoost and SimpleBinaryClassifier do not take into account unlabeled points (they assume they are negative points).

### Datasets

*Synthetic dataset created with function ``generate_dummy_dataset`` in ``stanscofi.datasets`` and the following arguments:
```python
npositive=200 #number of positive pairs
nnegative=100 #number of negative pairs
nfeatures=50 #number of pair features
mean=0.5 #mean for the distribution of positive pairs, resp. -mean for the negative pairs
std=1 #standard deviation for the distribution of positive and negative pairs
random_seed=124565 #random seed
```

**[a]** Réda, Clémence. (2023). TRANSCRIPT drug repurposing dataset (2.0.0) [Data set]. Zenodo. doi:10.5281/zenodo.7982976

**[b]** Gottlieb, A., Stein, G. Y., Ruppin, E., & Sharan, R. (2011). PREDICT: a method for inferring novel drug indications with application to personalized medicine. Molecular systems biology, 7(1), 496.

**[c]** Luo, H., Li, M., Wang, S., Liu, Q., Li, Y., & Wang, J. (2018). Computational drug repositioning using low-rank matrix approximation and randomized algorithms. Bioinformatics, 34(11), 1904-1912.

**[d]** Réda, Clémence. (2023). PREDICT drug repurposing dataset (2.0.1) [Data set]. Zenodo. doi:10.5281/zenodo.7983090

**[e]** Liang, X., Zhang, P., Yan, L., Fu, Y., Peng, F., Qu, L., … & Chen, Z. (2017). LRSSL: predict and interpret drug–disease associations based on data integration using sparse subspace learning. Bioinformatics, 33(8), 1187-1196.
