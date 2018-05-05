# NVM Practice

https://github.com/creationix/nvm

## [Installation](https://github.com/creationix/nvm#installation)

Using cURL:
```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.6/install.sh | bash
```

or Wget:
```bash
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.6/install.sh | bash
```

To verify that nvm has been installed, do:
```bash
command -v nvm
```
which should output 'nvm' if the installation was successful. Please note that `which nvm` will not work, since `nvm` is a sourced shell function, not an executable binary.

## [Usage](https://github.com/creationix/nvm#usage)

```bash
$ nvm

Note: <version> refers to any version-like string nvm understands. This includes:
  - full or partial version numbers, starting with an optional "v" (0.10, v0.1.2, v1)
  - default (built-in) aliases: node, stable, unstable, iojs, system
  - custom aliases you define with `nvm alias foo`

Usage:
  nvm install [-s] <version>                Download and install a <version>, [-s] from source. Uses .nvmrc if available
  nvm uninstall <version>                   Uninstall a version
  nvm use [--silent] <version>              Modify PATH to use <version>. Uses .nvmrc if available
  nvm current                               Display currently activated version
  nvm ls                                    List installed versions
  nvm ls-remote                             List remote versions available for install
  nvm deactivate                            Undo effects of `nvm` on current shell
  nvm unload                                Unload `nvm` from shell
  nvm which [<version>]                     Display path to installed node version. Uses .nvmrc if available
```