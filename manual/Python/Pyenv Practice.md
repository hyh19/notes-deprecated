# Pyenv Practice

https://github.com/pyenv/pyenv

## Installation

### [The automatic installer](https://github.com/pyenv/pyenv-installer)

- Prerequisites

  Refer to **Python Practice**

- Install
```
$ curl -L https://raw.githubusercontent.com/pyenv/pyenv-installer/master/bin/pyenv-installer | bash
```

- Add 'pyenv' to the load path
```
$ echo 'export PATH="/root/.pyenv/bin:$PATH"' >> ~/.bash_profile
$ echo 'eval "$(pyenv init -)"' >> ~/.bash_profile
$ echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bash_profile
$ source ~/.bash_profile
```

- Verifying
```
$ pyenv
```

- Show help message
```
$ pyenv -h|--help
```

### Plugins

[pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv)

[pyenv-virtualenvwrapper](https://github.com/pyenv/pyenv-virtualenvwrapper)

## [Command Reference](https://github.com/pyenv/pyenv/blob/master/COMMANDS.md)

List all available versions
```
$ pyenv install -l|--list
```

Install a Python version
```
$ pyenv install 3.6.1 -v
```

List all Python versions available to pyenv
```
$ pyenv versions
```

Set or show the global Python version
```
$ pyenv global 3.6.1
$ pyenv global
```

Set or show the local application-specific Python version
```
$ pyenv local 3.4.6
$ pyenv local
```

Unset the local version
```
$ pyenv local --unset
```

Show the current Python version and its origin
```
$ pyenv version
```

Uninstall a specific Python version
```
$ pyenv uninstall 3.6.1
```