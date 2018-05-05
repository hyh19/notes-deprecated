# Git Practice

https://git-scm.com/

https://git-scm.com/doc

https://git-scm.com/book/en/v2

## Git Cheat Sheets

[English](https://services.github.com/on-demand/downloads/github-git-cheat-sheet/)

[中文](https://services.github.com/on-demand/downloads/zh_CN/github-git-cheat-sheet/)

## [Installation](https://git-scm.com/downloads)

### [Source](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

[install-git-from-source.sh](https://github.com/mrhuangyuhui/git/blob/master/install-git-from-source.sh)
```bash
## CentOS 6.9/7.4, Ubuntu 16.04 ##
curl -L https://github.com/mrhuangyuhui/git/raw/master/install-git-from-source.sh | bash
```

```bash
## CentOS ##
yum install epel-release -y
yum install wget tar autoconf automake libtool -y
yum install dh-autoreconf libcurl-devel expat-devel gettext-devel openssl-devel perl-devel zlib-devel -y
yum install asciidoc xmlto docbook2X -y

## Ubuntu ##
apt-get install wget tar
apt-get install dh-autoreconf libcurl4-gnutls-dev libexpat1-dev  gettext zlib1g-dev libssl-dev -y
apt-get install asciidoc xmlto docbook2x -y


ln -s /usr/bin/db2x_docbook2texi /usr/bin/docbook2x-texi

wget https://www.kernel.org/pub/software/scm/git/git-2.15.0.tar.gz
tar -zxf git-2.15.0.tar.gz
cd git-2.15.0
make configure
./configure --prefix=/usr/local/git/git-2.15.0
make all doc info
make install install-doc install-html install-info
```

## [Reference](https://git-scm.com/docs)

### Guides

#### [gitignore](https://git-scm.com/docs/gitignore)

https://help.github.com/articles/ignoring-files/

https://github.com/github/gitignore

https://gist.github.com/octocat/9257657

## [GitHub](https://github.com/)

https://help.github.com/

## [GUI Clients](https://git-scm.com/downloads/guis)




