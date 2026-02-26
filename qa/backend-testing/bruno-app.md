# Bruno

#### links

- [doc](https://docs.usebruno.com)
- [cli](https://www.npmjs.com/package/@usebruno/cli)

## response data

src - https://github.com/usebruno/bruno/blob/main/packages/bruno-js/src/bruno-response.js

```js
res
res.status
res.statusText
res.headers
res.body
res.responseTime

getStatus()
getHeader(name)
getHeaders() 
getBody()
getResponseTime() 
setBody(data) 
```

## variables

### runtime

 runtime variables can only be set via the API - `getVar()` and `setVar()` or via Vars/Post-response request menu

## Structure

`.env`

stores all environmental vars. For env-specific vars name should have `_ENV` on the end like `_STAGE`

```.env
# Enviroment

## Section/Group

ITEM=one
VAR=123
ETC=etc

# Prod
## Url

URL_LOGIN_PROD=login.prod
URL_STORE_PROD=store.prod

## Token

## Login variables
```

`enviroments/prod.bru`

list should consist of env important vars. Without env. info in name. like `userToken`, `loginData`.  

Most of the items in list will be duplicated due to reuse in different envs. prod and stage lists both will have `userToken` but with value `{{process.env.TOKEN_PROD}}` and `{{process.env.TOKEN_STAGE}}` accordingly

```.bru
vars {
  ~internalApiUrl: api.url
  internalApiUrl: api.v2.url
  urlLogin: {{process.env.URL_LOGIN_PROD}}
  urlStore: {{process.env.URL_STORE_PROD}}
  toggleToken: toggle
  ~toggleToken: token
  token: {{process.env.TOKEN_PROD}}
}
```

`collection.bru`

Will have all vars, scripts, and tests that should be used for all requests in collection

example:

- auth data
- regular response tests such as 2xx check and latency test

`request.bru`

- should consist of specific request data and specific request date
- should be ready for environment switch
- should be ready for request-specific data switch

## assert

can be used with [[#response data]]

should check response-specific data, otherwise we should use Test on Collection level

`request.bru`

```bru
assert {
  res.status: eq 200
  res.body.access_token: isNotEmpty
}
```

### array

in case if response is array

`res.body[0].id`

```json
[
{"bla": "blabla"},
{"id": "myId"}
]
```

## vars

helps to save and reuse response data

`request.bru`

```bru
vars:post-response {
  loginServerToken: res.body.access_token
}
```

## CLI

### install

```shell
npm install -g @usebruno/cli
```

### run

```shell
npx bru run

npx bru run foldername/testFolderName -r --env stage --reporter-html --insecure

# --env-var expects lovercase 
# --env-var  urlstage=https://$${CI_PROJECT_NAME}-$${CI_COMMIT_REF_SLUG}.$${k8s_base_domain}

```

### CI

#### gitlab

##### one job

```yml
image: node:24-alpine

stages:
  - test

run_bruno_tests:
  stage: test
  before_script:
    - apk add --no-cache make
    - corepack use pnpm@latest-10
    - npm ci # expect you to have @usebruno/cli in package.json
    `- echo "node $(node -v) bru $(npx bru --version)"`
  variables:
    URL_STAGE: "api.stage.srv.local"
  script:
    - echo "test will be conducted on $URL_STAGE"
    - cp .env.sample .env
    - make run_stage
  cache:
    key:
      files:
        - testCollection/package-lock.json
    paths:
      - testCollection/node_modules/
  artifacts:
    reports: # allows to see the test results in the GitLab UI
      junit: api-collection/bruno-report/report.xml
    paths: # allows to download the test results
      - api-collection/bruno-report
    when: on_failure
    expire_in: 1 day
```

##### multiple jobs

```yaml
# Templates
.bruno_template:
  image: node:24-alpine
  before_script:
    - apk add --no-cache make
    - cd testCollection
    - npm ci
    - echo "node $(node -v) npm $(npm -v) bru $(npx bru --version)"
    - cp .env.sample .env
    - sed -i '/^URL_STAGE=/d' .env
  cache:
    key:
      files:
        - testCollection/package-lock.json
    paths:
      - testCollection/node_modules/
  artifacts:
    reports:
      junit: testCollection/bruno-report/report.xml
    paths:
      - estCollection/bruno-report
    when: on_failure
    expire_in: 1 day

# Jobs

bruno_current_tests:
  stage: test
  extends:
    - .only_template
    - .bruno_template
  when: manual
  allow_failure: true
  variables:
    URL_STAGE: "loyalty-points-api.gcp-k8s-b2c-login-stage.srv.local"
  script:
    - echo "test will be conducted on $URL_STAGE"
    - make run_stage_ci_current


bruno_deployment_tests:
  stage: test_on_stage
  extends:
    - .k8s_variables_stage
    - .bruno_template
  variables:
    URL_STAGE: "$CI_PROJECT_NAME-$CI_COMMIT_REF_SLUG.stage.srv.local"
  script:
    - echo "test will be conducted on $URL_STAGE"
    - make run_stage_ci_deployment
    
  rules:
    - if: '$CI_COMMIT_TAG =~ /^stage-.*$/'
```

### make

because its more convenient than package.json scripts

```makefile
run_stage:
	test -d bruno-report || mkdir bruno-report
	npx bru run Api/testCollection \
		-r \
		--env stage \
		--reporter-html bruno-report/report.html \
		--reporter-junit bruno-report/report.xml \
		--insecure
```

```makefile
TIMESTAMP := $(shell date +%Y%m%d-%H%M%S)
SUITE_NAME = $(shell basename $(SUITE_PATH))
REPORT_DIR = brunoReport/$(ENV)/$(SUITE_NAME)
REPEAT = 1

run_suite:
	@mkdir -p $(REPORT_DIR)
	npx bru run $(SUITE_PATH) \
		-r \
		--env $(ENV) \
		--reporter-html $(REPORT_DIR)/$(TIMESTAMP)-report.html \
		--reporter-junit $(REPORT_DIR)/$(TIMESTAMP)-report.xml \
		--iteration-count $(REPEAT)\
		--insecure

run_suite_ci:
	@mkdir -p brunoReport
	npx bru run $(SUITE_PATH) \
		-r \
		--env $(ENV) \
		--reporter-html brunoReport/report.html \
		--reporter-junit brunoReport/report.xml \
		--iteration-count $(REPEAT)\
		--insecure

run_dev_userFlows_auth:
	@$(MAKE) run_suite SUITE_PATH=userFlows/auth ENV=dev

run_stage_userFlows_auth:
	@$(MAKE) run_suite SUITE_PATH=userFlows/auth ENV=stage

run_dev_requests:
	@$(MAKE) run_suite SUITE_PATH=requests ENV=dev

run_dev_ci:
	@$(MAKE) --no-print-directory run_suite_ci SUITE_PATH=requests/iamService/api-iam-health.bru ENV=dev

run_flow_auth_loginExistUsr:
	@$(MAKE) --no-print-directory run_suite_ci SUITE_PATH=userFlows/auth/email/loginExistUsr ENV=dev
	@$(MAKE) --no-print-directory run_suite_ci SUITE_PATH=userFlows/auth/email/loginExistUsr ENV=stage

run_flow_migrationPlugin_nonexistentLoginUser:
	@$(MAKE) --no-print-directory run_suite SUITE_PATH=userFlows/migrationPlugin/email/nonexistentLoginUser ENV=stage REPEAT=5
```

## example readme

```
# API — Bruno Collection

## deps

### App

- bruno v3.1.4

### CLI-Tool

- bruno-cli v3.1.3
- pnpm v10

## How to use UI

- install [Bruno](https://github.com/usebruno/bruno/releases) (use version from deps section)
- open collection via Bruno app
- set up secret environment variables

```sh
cp .env.example .env
# add your variables based on .env.example
```
```
## How to use CLI

### Deps

- Node (nvm + pnpm)
- Make

### Steps

```sh
# install Node
nvm install

# install pnpm via corepack
corepack use pnpm@latest-10

# set up env variables
cp .env.example .env

# run test/folder
npx bru run foldername/testFolderName -r --env stage --reporter-html --insecure
```
```
### Make

- shortcut to run default tests

```sh
# run default collection
make run_dev_requests
```
```
## Project

### Structure

#### Original requests

- requests

#### Tests

- tests
  - testName
    - deps (if any)
    - testName

- userFlows
  - flowName
    - deps (if any)
    - flowName

#### Bruno folders

- environments

#### Custum Folders

- brunoReport

#### Sensitive data

- .env

#### Deps

- `package.json`, `.nvmrc` - Node tooling config

### Rules

CodeStyle - camelCase

examples

- `urlInternalApi`
- `userId`
- `postLoginOtpConfirm.bru`
- `getUserData.bru`

```
