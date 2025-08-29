# 🦊 GitLab CI/CD

A pipeline is composed of independent jobs that run scripts, grouped into stages. Stages run in sequential order, but jobs within stages run in parallel.

#### Links

- doc - [https://docs.gitlab.com/ee/ci/](https://docs.gitlab.com/ee/ci/)
- examples - [https://docs.gitlab.com/ee/ci/examples/](https://docs.gitlab.com/ee/ci/examples/)
- guides - [https://gitlab.com/gitlab-org/gitlab-foss/-/blob/master/doc/development/cicd/templates.md](https://gitlab.com/gitlab-org/gitlab-foss/-/blob/master/doc/development/cicd/templates.md#development-guide-for-gitlab-cicd-templates)

## gitlab-ci.yml

[YAML file](https://docs.gitlab.com/ee/ci/yaml/), located in project's root directory, defines CI/CD pipeline, including stages, jobs, and runners.

```
.gitlab-ci.yml
```

### templates

- [link](https://docs.gitlab.com/ci/examples/#cicd-templates)

Use the `include` keyword to include the template.

### File Structure

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

## Gitlab Runner

Executes CI/CD jobs on your infrastructure (e.g. physical machines, virtual machines, Docker containers, or Kubernetes clusters).

## Stages

Stages define the order of execution for jobs

```bash
stages:
    - deps
    - test
    - build
    - e2e
    - deploy
```

## Jobs

Individual units of work within a stage. Jobs within stages run in parallel.

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

### when

by default jobs are automatic

- There's no `when: manual` setting
- There are no conditional `rules` that would prevent automatic execution
- There are no `only`/`except` clauses limiting execution

```yml
run_bruno_tests:
  stage: test
  when: manual  # This makes the job require manual intervention
  before_script:
    # ...rest of your configuration...
```

### only

```yml
run_bruno_tests:
  only:
    - main  # Only run automatically on the main branch
```

## Variables

Dynamic key-value pairs defined at different levels within GitLab environment (project, group, or instance).

- [doc](https://docs.gitlab.com/ci/variables)
- [predefined vars](https://docs.gitlab.com/ci/variables/predefined_variables/)

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

```yaml
# build, and push to registry
build_image:
  script:
    - docker build --tag ptrpl4/repo-name:ci-cd-test-app-1.0 .
    - docker login --user $DOCKER_USER --password $DOCKER_PASSWORD
    - docker push ptrpl4/repo-name:ci-cd-test-app-1.0
```

### Bruno

[bruno-app](../../qa/backend-testing/bruno-app.md)
