==============
single-version
==============

.. image:: https://madewithlove.now.sh/vn?heart=true&colorA=%23ffcd00&colorB=%23da251d
.. image:: https://badgen.net/pypi/v/single-version

Utility to let you have a single source of version in your code base.

This utility targets modern Python projects which have layout generated by `Poetry`_, with a *pyproject.toml* file in place of *setup.py*. With this layout, the project initially has two places to maintain version string: one in *pyproject.toml* file and one in some *\*.py* file (normally *__init__.py*):

.. code-block:: toml

    # pyproject.toml
    [tool.poetry]
    name = "your-package"
    version = "0.1.0"

.. code-block:: python

    # your_package/__init__.py
    __version__ = "0.1.0"

This duplicity often leads to inconsistency when you, the author, forget to update both.

*single-version* is born to solve that headache circumstance. By convention, it chooses the *pyproject.toml* file as original source of version string. Your project's ``__version__`` variable then is computed from it. When your package is already deployed and installed to some system, the version string will be retrieved from that Python environment (the *pyproject.toml* is not included in distribution file).

Years ago, to retrieve version for an installed package, ones often used `pkg_resources`_, which has well-known issue of causing slow import. Learning from that mistake, *single-version* use |importlib.metadata|_, which becomes standard from Python 3.8, instead.


Usage
-----

Add ``single_version`` as your project dependency:

.. code-block:: sh

    poetry add single-version


Assume that your package source tree is like this::

    .
    ├── awesome_name
    │  └── __init__.py
    ├── pyproject.toml
    ├── README.rst
    └── tests/

where the ``__version__`` variable is defined in `awesome_name/__init__.py` file. The file content can be like this:

.. code-block:: python

    from pathlib import Path

    from single_version import get_version


    __version__ = get_version('awesome_name', Path(__file__).parent.parent)


API Reference
-------------


*def* **get_version** (package_name: *str*, looked_path: *Path*) -> *str*

- ``package_name``:  Your package's name (same as in *pyproject.toml* file).

- ``looked_path`` (of ``pathlib.Path`` type): Folder where your project's *pyproject.toml* resides.


.. _Poetry: https://python-poetry.org/
.. _pkg_resources: https://setuptools.readthedocs.io/en/latest/pkg_resources.html
.. |importlib.metadata| replace:: ``importlib.metadata``
.. _importlib.metadata: https://docs.python.org/3.8/library/importlib.metadata.html
