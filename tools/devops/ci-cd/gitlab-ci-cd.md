# 🦊 GitLab CI/CD

## Info

A pipeline is composed of independent jobs that run scripts, grouped into stages. Stages run in sequential order, but jobs within stages run in parallel.

#### Links

doc - [https://docs.gitlab.com/ee/ci/](https://docs.gitlab.com/ee/ci/)

examples - [https://docs.gitlab.com/ee/ci/examples/](https://docs.gitlab.com/ee/ci/examples/)

guides - [https://gitlab.com/gitlab-org/gitlab-foss/-/blob/master/doc/development/cicd/templates.md](https://gitlab.com/gitlab-org/gitlab-foss/-/blob/master/doc/development/cicd/templates.md#development-guide-for-gitlab-cicd-templates)

#### Filename

```
.gitlab-ci.yml
```

#### File Structure

```yaml
# List of stages for jobs, and their order of execution
stages:          
  - build
  - test
  - deploy
# This job runs in the build stage, which runs first.
build-job:
  image: python:3.9       
  stage: build
  before_script:
    - apt-get update && apt-get install make
  script:
    - echo "Compiling the code..."
    - echo "Compile complete."
    - make build

# This job runs in the test stage.
unit-test-job:   
  stage: test    # It only starts when the job in the build stage completes successfully.
  script:
    - echo "Running unit tests... This will take about 60 seconds."
    - sleep 60
    - echo "Code coverage is 90%"

# This job also runs in the test stage.
lint-test-job:   
  stage: test    # It can run at the same time as unit-test-job (in parallel).
  script:
    - echo "Linting code... This will take about 10 seconds."
    - sleep 10
    - echo "No lint issues found."

# This job runs in the deploy stage.
deploy-job:      
  stage: deploy  # It only runs when *both* jobs in the test stage complete successfully.
  environment: production
  script:
    - echo "Deploying application..."
    - echo "Application successfully deployed.
```

## Jobs

### Job name limitations

You can’t use these keywords as job names:

* `image`
* `services`
* `stages`
* `types`
* `before_script`
* `after_script`
* `variables`
* `cache`
* `include`
* `true`
* `false`
* `nil`

Job names must be 255 characters or fewer.

Use unique names for your jobs. If multiple jobs have the same name, only one is added to the pipeline, and it’s difficult to predict which one is chosen.

## Variables

```yaml
build_image:
  variables:
    IMAGE_NAME: ptrpl4/repo-name
    IMAGE_TAG: ci-cd-test-app-1.0
  before_script:
    - docker login -u $DOCKER_USER -p $DOCKER_PASSWORD
  script:
    - docker build -t $IMAGE_NAME:$IMAGE_TAG .
    - docker push $IMAGE_NAME:$IMAGE_TAG
```

## Stages

```bash
stages:
    - deps
    - test
    - build
    - e2e
    - deploy
```

## Services

You can specify an additional image by using the `services` keyword. This additional image is used to create another container, which is available to the first container. The two containers have access to one another and can communicate when running the job.

doc - [https://docs.gitlab.com/ee/ci/services/](https://docs.gitlab.com/ee/ci/services/)

ding = docker in docker

```yaml
build_image:
  image: docker:20.10.18
  services:
    - docker:20.10.18-ding
  variables:
    DOCKER_TLS_CERTDIR: "/certs"
```

## Examples

### Docker

doc - [https://app.gitbook.com/s/-LcfwXJnDmljkkxrOjKD/\~/changes/5SZzGUfIdp4URfMFYILq/tools/ci-cd/gitlab-pipelines#info](gitlab-ci-cd.md#info)

```yaml
# build, and push to registry
build_image:
  script:
    - docker build --tag ptrpl4/repo-name:ci-cd-test-app-1.0 .
    - docker login --user $DOCKER_USER --password $DOCKER_PASSWORD
    - docker push ptrpl4/repo-name:ci-cd-test-app-1.0

```
