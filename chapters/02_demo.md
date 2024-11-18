(sec-python-packaging)=
# Python Packaging

Why package your code?

* You can use the code from anywhere on your computer (less messing with file
  paths)
* Easier to deploy on other computers or share with collaborators
* Ready to go if you decide to distribute the package


Creating the Package
--------------------

:::{note}
This is just one way to create a package. There are others---see the references
in {numref}`references`.
:::

To create a package, you need to:

1. Set up a directory named after the package for the package code. If you've
   been using `src/`, it's usually a good idea to make a subdirectory there. So
   if the package is called `dpug`, make `src/dpug` and put the code there.
2. Create a file `__init__.py` in each subdirectory of the package. For
   example, in `dpug/__init__.py`. This file can be empty or a module to import
   when the package/directory is imported (for example, when the user runs
   `import dpug`).
3.  Create a `pyproject.toml` file in the same directory as `src/`. There are
    many ways to customize this file (and your package). A minimal example is:

    ```toml
    # Package metadata
    [project]
    name = "dpug"
    version = "0.0.1"
    authors = [
      { name = "Nick Ulle", email = "nick.ulle@gmail.com" },
    ]
    description = "DPUG Demo Package"
    readme = "README.md"
    requires-python = ">=3.10"

    # Where to find the package code (if the package directory is in another
    # directory).
    [tool.setuptools.packages.find]
    where = ["src"]

    # How to build the package. Use setuptools unless you know you need
    # something else.
    [build-system]
    requires = ["setuptools"]
    build-backend = "setuptools.build_meta"
    ```


Installing the Package
----------------------

If you want to use the package while you're working on it, create an **editable
install**. To do this, open a terminal at the top level project directory,
activate the conda environment you use for the project, and then run:

```sh
pip install -e .
```

This command creates soft links (symlinks) to the code in the package. Any
changes you make to the code will be reflected in the installed package
*without the need to reinstall*, although you may have to restart Python.

If you want a regular install (non-editable), leave out the `-e`. This will
copy the files rather than creating soft links.



Using the Package
-----------------

You can import the package with:

```python
import dpug
```

The package should also appear as `dpug` in the output of `pip list`.

:::{tip}
If you use an editable install and IPython or Jupyter, the [autoreload][]
extension can automatically reload packages as you modify them (most of the
time). The extension is built-in; you can enable it by running these commands
in an IPython or Jupyter session:

```
%load_ext autoreload
%autoreload 2
```

If you change a file and find that the extension doesn't detect it, try saving
the file and running the associated `import` line again.
:::


(references)=
References
----------

* The [Python Packaging User Guide][pypa-guide], by the developers of `pip` and
  other packaging tools
* The [pyOpenSci Python Package Guide][pyos-guide]
* [Python Packages][py-packages] by Beuzen & Timbers, a CRC Press book
* [Traps for the Unwary in Python’s Import System][import-traps] by Coghlan

[pypa-guide]: https://packaging.python.org/en/latest/
[pyos-guide]: https://www.pyopensci.org/python-package-guide/
[py-packages]: https://py-pkgs.org/welcome
[import-traps]: https://python-notes.curiousefficiency.org/en/latest/python_concepts/import_traps.html
