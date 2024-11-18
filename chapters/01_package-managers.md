# Package Managers

A **package manager** is a tool that downloads and installs software packages.
In the context of Python, these are usually Python packages.

Different projects often require different computing **environments**, with
different software and software versions. If you work on multiple projects, the
requirements might conflict. For instance, you might be using Python 3.13 for
your newest project, but also have an older project that requires Python 3.8.

Many package managers provide a way to create **virtual environments**, which
are separate, independent environments. You can install different software and
software versions into each virtual environment.

Python's official package manager is called pip and is relatively minimalistic.
With pip, you can install packages from the [PyPI][], the primary repository
for community-contributed Python packages. You can also use pip to install a
package from a file.

[PyPI]: https://pypi.org/

Compared to pip, other package managers provide extra features, such as virtual
environment management, package creation, and better performance. Many
different package managers exist.

Among researchers, [conda][] is an especially popular package manager. In
contrast to pip, conda can install many kinds of software, not just Python
packages. Many different repositories (or "channels") provide conda packages,
but the most widely-used are `defaults`, which is maintained by [Anaconda][] (a
private company), and [conda-forge][], which is maintained by the community.

[conda]: https://conda.org/
[Anaconda]: https://anaconda.org/
[conda-forge]: https://conda-forge.org/

I recommend and use [uv][] and [pixi][] for package management. These are
described in the next two sections.

[uv]: https://docs.astral.sh/uv/
[pixi]: https://pixi.sh/

## uv

[uv][] is a fast replacement for pip. Besides installing PyPI packages, uv
provides environment and project management features. I usually don't use uv
directly, because I use a lot of conda-forge packages and [pixi](#pixi) is a
wrapper for uv. However, there are a few uv commands that are useful on their
own.

If you want to set up a new Python project, especially if you might turn the
code into a package, you can create the initial files by running:

```sh
uv init
```

This will create 3 files:

1. `README.md`, intended as the first file people will read at when they look
   at your project. Put documentation about the project here.

2. `pyproject.toml`, the [modern, official standard][pep-518] for metadata
   about Python projects and packages.

3. `hello.py`, a Python script that prints "Hello!".

[pep-518]: https://peps.python.org/pep-0518/


If you've set up your Python project as a package (with appropriate directories
and `__init__.py` files), you can build the package into a binary **wheel
file** (`*.whl`) with:

```sh
uv build
```

More about packaging in {numref}`sec-python-packaging`.


(pixi)=
## pixi

[pixi][] is a fast replacement for conda. Besides installing conda packages,
pixi provides environment management and project management features. Pixi is
built on uv, so you also use pixi to install PyPI packages. Pixi is a
relatively new tool, so some features are still in development.

Pixi is project-oriented, so packages are stored in a hidden `.pixi/` directory
in each project's directory. You can initialize pixi for a project by running:

```sh
pixi init
```

This will create a `pixi.toml` file with metadata about your project, unless
you already have a `pyproject.toml` file, in which case pixi will prompt you
about whether you want to use that instead. If you're planning to package your
project, it's a good idea to use `pyproject.toml`; otherwise, use `pixi.toml`.

When run in a git repository, the `pixi init` command will also update or
create two other files: `.gitignore` and `.gitattributes`. These files contain
configuration for git. Pixi updates them so that certain pixi files which
should not be stored in version control, such as the `.pixi/` directory, are
ignored.

You can add dependencies to your project by running the `pixi add` command
anywhere in your project directory (including subdirectories). By default,
these are dependencies for packages on conda-forge. For instance, to add
pandas, run:

```sh
pixi add pandas
```

If you want to add a dependency on a package from PyPI (that is, a package
installed with pip), use `pixi add` with the `--pypi` flag. For example, to add
the PyPI version of matplotlib, run:

```sh
pixi add --pypi matplotlib
```

If you're planning to package your project for distribution on PyPI, it's
currently best to add only PyPI dependencies, since PyPI and pip are not
conda-aware. In the future, pixi will add support for packaging projects, and
will likely provide a way to generate PyPI packages even for projects that
depend on packages on conda-forge. If you are not going to distribute your
project on PyPI, it's generally better to use packages from conda-forge, since
a wider variety of packages are available.

:::{note}
You can use pixi to install packages from other repositories besides
conda-forge. See the documentation for details.
:::

When you run `pixi add`, pixi will automatically update your `pixi.toml` or
`pyproject.toml` file. It will also create or update another file, `pixi.lock`,
which contains specific version information for each added package.

:::{tip}
If you use version control such as git, the pixi documentation recommends that
you commit both `pixi.toml`/`pyproject.toml` and `pixi.lock`.
:::

Pixi creates a virtual environment automatically when you add dependencies to a
project. You can open a shell in the virtual environment by running:

```sh
pixi shell
```

When you're finished using the virtual environment, you can exit with the
`exit` command.

If you just want to run a single command in the virtual environment, you can
use `pixi run` instead of `pixi shell`. Put the command you want to run after
`pixi run`. For instance, to open Python in the virtual environment:

```sh
pixi run python
```

:::{note}
You can use pixi to create multiple virtual environments for a project. See the
documentation for details.
:::

One of the major features of pixi is that it can reliably recreate a virtual
environment from `pixi.toml`/`pyproject.toml` and/or `pixi.lock`. After getting
a copy of a project with these files, you can recreate the project's virtual
environment by running:

```sh
pixi install
```

:::{tip}
If you want to use a project's virtual environment on multiple operating
systems, you'll need to list these in `pixi.toml`/`pyproject.toml`. Common
values are `linux-64`, `osx-64`, and `win-64`.
:::

Pixi has many more features! For instance, you can use pixi as a task runner
(similar to but simpler than make or snakemake). Take a look at the
documentation for details.

