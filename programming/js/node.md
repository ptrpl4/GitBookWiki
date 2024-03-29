---
description: Asynchronous event-driven JavaScript runtime
---

# NodeJS

### Links

* Docs - [https://nodejs.org/en/](https://nodejs.org/en/)
* n - [https://github.com/tj/n](https://github.com/tj/n)
* nvm - [https://github.com/nvm-sh/nvm](https://github.com/nvm-sh/nvm)
* how to update node - [https://guillermo.at/update-node-proper-way](https://guillermo.at/update-node-proper-way)

### Manage Node versions

#### with n

```bash
# install with n (Node.js version management)
npm install -g n
# install node with n
n lts # note! on macOS you probably should change app directory to avoid r/w restrictions
```

#### with nvm

```bash
# install nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
# check
nvm --version
# instal node
nvm install 16
# check installed
nvm ls
# choose installed
nvm use 16
# uninstall
nvm uninstall 8
```

### process.env

Returns an object containing the user environment

docs - [https://nodejs.org/docs/latest/api/process.html#process\_process\_env](https://nodejs.org/docs/latest/api/process.html#process\_process\_env)

## JS modules

### dotenv

Loads environment variables from a .env file into process.env

docs - [https://github.com/motdotla/dotenv#dotenv](https://github.com/motdotla/dotenv#dotenv-)

```javascript
require('dotenv').config()
// import 'dotenv/config' // for ES6

console.log(process.env)
```
