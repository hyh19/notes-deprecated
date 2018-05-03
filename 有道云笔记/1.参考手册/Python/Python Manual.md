[TOC]

# Python Manual

https://www.python.org

https://www.python.org/doc/

https://github.com/python/cpython

## Installation

Prerequisites(CentOS 7)
```
$ yum group install 'Development Tools' -y
$ yum install zlib-devel bzip2-devel openssl-devel ncurses-devel readline-devel sqlite-devel -y
```

### Using [pyenv](https://github.com/pyenv/pyenv) (Recommended)

### Source code

Download

https://www.python.org/downloads/

Build
```
$ ./configure --prefix=/usr/local/Python/Python-3.6.2/
$ make
$ make test
$ make install
```

Symlink
```
$ cd /usr/local/Python/
$ ln -s Python-3.6.2/ default
```

Add to the load path
```
$ echo 'export PATH=/usr/local/Python/default/bin:$PATH' >> ~/.bash_profile
$ source ~/.bash_profile
```

Verifying
```
$ python3 -V
```

## References

[General Index](https://docs.python.org/3/genindex.html) \
[Global Module Index](https://docs.python.org/3/py-modindex.html) \
[Search page](https://docs.python.org/3/search.html)

http://devdocs.io/python~3.6/ \
http://devdocs.io/python~2.7/

**[sys](https://docs.python.org/3.6/library/sys.html)**

- [sys.modules](https://docs.python.org/3.6/library/sys.html#sys.modules)

### [Python 3](https://docs.python.org/3/)

## Tutorials

[The Python Standard Library](https://docs.python.org/3/library/index.html)

[The Python Language Reference](https://docs.python.org/3.6/reference/index.html)

## Related Practices

**PIP Practice**

**Virtualenv Practice**

**Pyenv Practice**