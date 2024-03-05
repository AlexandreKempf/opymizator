Opymizator
==========

|PyPI| |Status| |Python Version| |License|

|Tests| |Codecov| |pre-commit| |Black|

.. |PyPI| image:: https://img.shields.io/pypi/v/opymizator.svg
   :target: https://pypi.org/project/opymizator/
   :alt: PyPI
.. |Status| image:: https://img.shields.io/pypi/status/opymizator.svg
   :target: https://pypi.org/project/opymizator/
   :alt: Status
.. |Python Version| image:: https://img.shields.io/pypi/pyversions/opymizator
   :target: https://pypi.org/project/opymizator
   :alt: Python Version
.. |License| image:: https://img.shields.io/pypi/l/opymizator
   :target: https://opensource.org/licenses/MIT
   :alt: License
.. |Tests| image:: https://github.com/AlexandreKempf/opymizator/workflows/Tests/badge.svg
   :target: https://github.com/AlexandreKempf/opymizator/actions?workflow=Tests
   :alt: Tests
.. |Codecov| image:: https://codecov.io/gh/AlexandreKempf/opymizator/branch/main/graph/badge.svg
   :target: https://app.codecov.io/gh/AlexandreKempf/opymizator
   :alt: Codecov
.. |pre-commit| image:: https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit&logoColor=white
   :target: https://github.com/pre-commit/pre-commit
   :alt: pre-commit
.. |Black| image:: https://img.shields.io/badge/code%20style-black-000000.svg
   :target: https://github.com/psf/black
   :alt: Black


Features
--------

- optimize your next batch of hyperparameters 🎉
- model agnostic 🙈
- easy to use 🚀

Requirements
------------

- a computer 💻
- a model or a loss function 🛠️
- a set of hyperparameters to optimize 🎛️
- `optuna`_ and `pyyaml`_ 📦


Installation
------------

You can install *Opymizator* via pip_ from PyPI_:

.. code:: console

   $ pip install opymizator

or using pipx_:

.. code:: console

   $ pipx install opymizator


Motivation
----------

I often get myself into these situations:
 - working on R&D projects with a code that might not be as clean as I would like it to be.
 - training production models but each training as to be launch from a terminal or a GUI.
 - not using a programming language to train my model.

You might be happy with your model in these cases but you know it could be better
with a few hyperparameters tweaks. Unfortunately, you'll need to do it with instinct because
all these situations are not really compatible with a hyperparameters optimization library.

That's why I though about *Opymizator*.

The goal is to abstract the optimization process and make it as easy as possible to use.
No need to refacto your code, no need to use a specific programming language, no need to use a specific framework.


Usage
-----

You can have a look at the `example` directory for an example on how to use *Opymizator*.

Given a `trials.csv` file that looks like this:

.. code:: csv

   x,lr,optimizer,result
   1,0.1,adam,4
   2,0.1,sgd,3
   10,0.05,sgd,1
   4,0.00001,adam,2
   ,,,
   ,,adam,
   10,,,

you can run the following command:

.. code:: console

   $ opymizator optimize trials.csv


This will look at the parameters names and the previous trials (the first 5 lines).
It will consider the last column as a value to minimize and given that knowledge,
*Opymizator* will try to find the best hyperparameters to fill the blank in your `trials.csv` file.

The result will looks like this:

.. code:: csv

   x,lr,optimizer,result
   1,0.1,adam,4
   2,0.1,sgd,3
   10,0.05,sgd,1
   4,0.00001,adam,2
   5,0.0334,adam,
   2,0.00232,sgd,
   10,0.1,sgd,

All the parameters you pre-inputted are unchanged, and the hyperparameters you left blank were filled
thanks to the Optuna library. The range values for quantitative hyperparameters are find based on minimum
and maximum values for each column, and the choices for qualitative hyperparameters are find based on the
values you already used in the file.

Usage with config
-----------------

You can specify a configuration file to use with the `--config` option. There is an
example in the `example` directory.

The configuration file should be a YAML file with the following structure:

.. code :: yaml

   sampler:  # information relative to the optuna sampler, any optuna sampler is allowed
      function: TPESampler
      seed: 10  # random seed for the sampler.

   parameters:  
      optimizer:  # name of the parameter, should be the same as the csv file.
         type: str  # support str, int, and float.
         choices: ["adam", "sgd"]  # for str type, the possible values the hyperparameter can take.
      lr:
         type: float
         low: 0.00001  # for float or int type, the minimum value the hyperparameter can take.
         high: 0.1  # for float or int type, the maximum value the hyperparameter can take.
         log: True  # a flag to sample the value from the log domain or not.
      x:
         type: int
         low: 1
         high: 10


Contributing
------------

Contributions are very welcome.
To learn more, see the `Contributor Guide`_.


License
-------

Distributed under the terms of the `MIT license`_,
*Opymizator* is free and open source software.


Issues
------

If you encounter any problems,
please `file an issue`_ along with a detailed description.


.. _MIT license: https://opensource.org/licenses/MIT
.. _PyPI: https://pypi.org/
.. _file an issue: https://github.com/AlexandreKempf/opymizator/issues
.. _pip: https://pip.pypa.io/
.. _optuna: https://github.com/optuna/optuna
.. _pyyaml: https://github.com/yaml/pyyaml
.. github-only
.. _Contributor Guide: CONTRIBUTING.rst
