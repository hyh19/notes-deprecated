# NPM Practice

https://www.npmjs.com/

## [CNPM](https://npm.taobao.org/)

```bash
npm config set registry https://registry.npm.taobao.org -g
```

```bash
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

## [Installation](https://www.npmjs.com/get-npm)

npm is distributed with Node.js- which means that when you download Node.js, you automatically get npm installed on your computer.

**Updating npm**
```bash
npm install npm@latest -g
```

## Usage

```bash
$ npm

Usage: npm <command>

npm <command> -h     quick help on <command>
npm -l           display full usage info
npm help <term>  search for help on <term>
npm help npm     involved overview
```

**CLI Commands**

[init](https://docs.npmjs.com/cli/init) \
[install](https://docs.npmjs.com/cli/install) \
[uninstall](https://docs.npmjs.com/cli/uninstall) \
[update](https://docs.npmjs.com/cli/update)

## [Documentation](https://docs.npmjs.com/)

### Getting Started

#### [Installing npm packages locally](https://docs.npmjs.com/getting-started/installing-npm-packages-locally)

```bash
npm install <package_name>
```

#### [Using a `package.json`](https://docs.npmjs.com/getting-started/using-a-package.json)

To create a `package.json` run:
```bash
npm init
```

You can get a default `package.json` by running `npm init` with the `--yes` or `-y` flag:
```bash
npm init --yes
```

You can also set several config options for the `init` command. Some useful ones:
```bash
npm set init.author.email "wombat@npmjs.com"
npm set init.author.name "ag_dubs"
npm set init.license "MIT"
```

To add an entry to your `package.json`'s `dependencies`:
```bash
npm install <package_name> --save
```

To add an entry to your `package.json`'s `devDependencies`:
```bash
npm install <package_name> --save-dev
```

#### [Updating local packages](https://docs.npmjs.com/getting-started/updating-local-packages)

To do this, run `npm update` in the same directory as your `package.json` file.

#### [Uninstalling local packages](https://docs.npmjs.com/getting-started/uninstalling-local-packages)


You can remove a package from your `node_modules` directory using `npm uninstall <package>`:
```bash
npm uninstall lodash
```

To remove it from the dependencies in `package.json`, you will need to use the `save` flag:
```bash
npm uninstall --save lodash
```

Note: if you installed the package as a `devDependency` (i.e. with `--save-dev`) then `--save` won't remove it from `package.json`. You have to use `--save-dev` to uninstall it.

#### [Installing npm packages globally](https://docs.npmjs.com/getting-started/installing-npm-packages-globally)

To download packages globally, you simply use the command `npm install -g <package>`, e.g.:
```bash
npm install -g jshint
```

#### [Updating global packages](https://docs.npmjs.com/getting-started/updating-global-packages)

To update global packages, you can use `npm update -g <package>`:
```bash
npm update -g jshint
```

To update all global packages, you can use `npm update -g`.

#### [Uninstalling global packages](https://docs.npmjs.com/getting-started/uninstalling-global-packages)

Global packages can be uninstalled with `npm uninstall -g <package>`:
```bash
npm uninstall -g jshint
```

## Configuring `npm`

### [`package.json`](https://docs.npmjs.com/files/package.json)

[devDependencies](https://docs.npmjs.com/files/package.json#devdependencies) \
[files](https://docs.npmjs.com/files/package.json#files) \
[keywords](https://docs.npmjs.com/files/package.json#keywords) \
[main](https://docs.npmjs.com/files/package.json#main) \
[repository](https://docs.npmjs.com/files/package.json#repository) \
[scripts](https://docs.npmjs.com/files/package.json#scripts)

