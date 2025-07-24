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

### Example 1

#### From bottom to top

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

### notes

- better to avoid manual sorting if it not necessary - all item in folder will update `seq` info if you insert new item in top of the list

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
bru run

bru run foldername/testFolderName -r --env stage --reporter-html --insecure

# --env-var expects lovercase 
# --env-var  urlstage=https://$${CI_PROJECT_NAME}-$${CI_COMMIT_REF_SLUG}.$${k8s_base_domain}

```

### CI

#### gitlab

##### one job

```yml
image: node:22-alpine

stages:
  - test

run_bruno_tests:
  stage: test
  before_script:
    - apk add --no-cache make
    - cd api-collection
    - npm ci # expect you to have @usebruno/cli in package.json
    - echo "node $(node -v) npm $(npm -v) bru $(npx bru --version)"
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
  image: node:22-alpine
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
