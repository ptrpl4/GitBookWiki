# NodeJS

Asynchronous event-driven JavaScript runtime.

#### Links

* Docs - [https://nodejs.org/en/](https://nodejs.org/en/)

## Node version management

### n

* n - [https://github.com/tj/n](https://github.com/tj/n)

```bash
# install with n (Node.js version management)
npm install -g n
# install node with n
n lts # note! on macOS you probably should change app directory to avoid r/w restrictions
```

### nvm

- [nvm](https://github.com/nvm-sh/nvm)

```bash
# choose folder
cd
mkdir nvm
NVM_DIR="${HOME}/nvm"

# install/update nvm (check latest https://github.com/nvm-sh/nvm/releases)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.2/install.sh | bash

# instal node
nvm install 18
nvm install --lts

# install project node from .nvmrc file in project dir
nvm install

# npm
nvm install-latest-npm

# check installed
nvm ls
nvm use 20

# choose installed
nvm use 18

# uninstall
nvm uninstall 12
```

## npm

Node Package Manager

#### Links

* Official - [npm](https://www.npmjs.com/)

### Installation

**npm** is installed with **Node.js**

```bash
# install/upgrade to latest
npm install -g npm

# check version
npm -v
```

### Usage

```bash
# init for new project
npm init

# (un)install
npm install express
npm uninstall package_name
npm install -g package_name

# update
npm update package_name@version
npm outdated -g --depth=0
npm update -g

# info
npm view npm

# install projects deps
npm install

# list
npm ls -g # global level

# config ( .npmrc file in - ~/ and project/ )
npm config list
npm config set fund false
npm config get fund

npm config -L project set fund false
```

### Installation folders

- local installs have links created at the `./node_modules/.bin/` directory
- global installs have links created from the global `bin/` directory (for example: `/usr/local/bin` on Linux or at `%AppData%/npm` on Windows)

### npx

Node Package eXecutor?

Package runner. Since npm version [5.2.0](https://github.com/npm/npm/releases/tag/v5.2.0) is pre-bundled with npm
- https://www.npmjs.com/package/npx

```bash
npx playwright
```

### npm Registry

Hosts public and private packages.

### package-lock.json

provides partial support for deterministic builds through the package-lock.json file

## yarn

Yet Another Resource Negotiator

* [yarn](https://yarnpkg.com/)

### Installation

```bash
# yarn installation
# if Node.js >=16.10
corepack enable
yarn set version stable
yarn -v
```

### Usage

```bash
# init for new project
yarn init

# Add/rm a package
yarn add package_name
yarn global add package_name

yarn remove package_name

yarn upgrade name@version

# info
yarn info name
```

### yarn.lock

generates a lockfile (**yarn.lock**) to ensure deterministic installations, guaranteeing consistent dependency resolution across different environments

## Package.JSON

The content of package.json must be written in **JSON**.

At least two fields must be present in the definition file: **name** and **version**.

```json
{
"name" : "foo",
"version" : "1.2.3",
"description" : "A package for fooing things",
"main" : "foo.js",
"keywords" : ["foo", "fool", "foolish"],
"author" : "John Doe",
"licence" : "ISC"
} 
```

### packages

A **package** is a file or directory that is described by a `package.json` file. A package must contain a `package.json` file in order to be published to the npm registry.

**Scoped** public packages belong to a user or organization and must be preceded by the user or organization name when included as a dependency in a `package.json` file:

* `@username/package-name`
* `@org-name/package-name`

### modules

A **module** is any file or directory in the `node_modules` directory that can be loaded by the Node.js `require()` function.

Since modules are not required to have a `package.json` file, not all modules are packages. Only modules that have a `package.json` file are also packages.

### devDependencies

deps only needed for development. Stored in devDependencies section in package.json

```shell
npm install --save-dev <package> # or -D

npm install --production # ignored all devDep
```

## process.env

Returns an object containing the user environment

- [docs](https://nodejs.org/docs/latest/api/process.html#process\_process\_env)

## JS modules

### dotenv

Loads environment variables from a .env file into process.env

docs - [https://github.com/motdotla/dotenv#dotenv](https://github.com/motdotla/dotenv#dotenv-)

```javascript
require('dotenv').config()
// import 'dotenv/config' // for ES6

console.log(process.env)
```
