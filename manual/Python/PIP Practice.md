# PIP Practice

https://pip.pypa.io/en/stable/

https://github.com/pypa/pip

https://pypi.python.org/pypi/pip

## [Installation](https://pip.pypa.io/en/stable/installing/)

### Installing with [get-pip.py](https://bootstrap.pypa.io/get-pip.py)
```
$ wget https://bootstrap.pypa.io/get-pip.py
$ python get-pip.py
```

### [Using Linux Package Managers](https://packaging.python.org/guides/installing-using-linux-tools/#installing-pip-setuptools-wheel-with-linux-package-managers)
```
$ yum install python-pip
$ apt-get install python-pip
```

Verifying
```
$ pip
```

Show help message
```
$ pip -h|--help
```

Show help for commands
```
$ pip help install
```

### Upgrading pip
- On Linux or macOS
```
$ pip install -U pip
```

- On Windows
```
$ python -m pip install -U pip
```

## [Quickstart](https://pip.pypa.io/en/stable/quickstart/)

Install a package from PyPI
```
$ pip install SomePackage
  [...]
  Successfully installed SomePackage
```

Show what files were installed
```
$ pip show --files SomePackage
  Name: SomePackage
  Version: 1.0
  Location: /my/env/lib/pythonx.x/site-packages
  Files:
   ../somepackage/__init__.py
   [...]
```

## [User Guide](https://pip.pypa.io/en/stable/user_guide/)

- ### [Installing Packages](https://pip.pypa.io/en/stable/user_guide/#installing-packages)

- ### [Listing Packages](https://pip.pypa.io/en/stable/user_guide/#listing-packages)

- ### [Searching for Packages](https://pip.pypa.io/en/stable/user_guide/#searching-for-packages)

## [Reference Guide](https://pip.pypa.io/en/stable/reference/)

- ### [pip install](https://pip.pypa.io/en/stable/reference/pip_install/)

- ### [pip list](https://pip.pypa.io/en/stable/reference/pip_list/)

- ### [pip show](https://pip.pypa.io/en/stable/reference/pip_show/)