# GitHub actions

links:
- [understanding-the-workflow-file](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions#understanding-the-workflow-file)

## Components

- Workflows - composed of jobs that can be started by an event or at a specific time. 
  Defined by a YAML file in the `.github/workflows` folder.
- Job - set of steps inside the same virtual machine runner, or container. 
  By default executed in parallel. Each step is a shell script or an _action_ that will be run. Steps are executed in order and are dependent on each other. Since each step is executed on the same runner, you can share data from one step to another.
- Events - triggers an execution of a Workflow. Internal GitHub event, a scheduled event (triggered at a specific time), or an external event (triggered by a webhook).
- Actions - (usually) created by public community snippets for solving popular tasks in Workflows.
- Runners - is a virtual machine that runs a job.

## Logic

### Artifacts

Artifacts are used for sharing data between jobs, it also saves data once the workflow is finished, also are generally used to save logs and reports of your workflow.

```yaml
# upload artifact
- name: Upload deployable package
  uses: actions/upload-artifact@v2
  with:
    name: production-files
    path: path/to/artifact

# dowload artifact
- name: Download artifact
  uses: actions/download-artifact@v2
  with:
    name: production-files
    path: path/to/artifact 
```


```yaml
name: Time artifact
on:
  push:
jobs:
  create:
    runs-on: ubuntu-latest
    steps:
      - name: Create a text file
        run: |
          echo "The time is $(date -u)" > time.txt

      - name: Upload time artifact
        uses: actions/upload-artifact@v3
        with:
          name: time-artifact # optional, default is artifact
          path: time.txt # required
          if-no-files-found: error # optional, default is warn
          retention-days: 1 # optional, maximum 90 days
          
      - name: Upload on failure 
        uses: actions/upload-artifact@v3 
        if: failure() 
        with: 
	      name: failure_artifact 
	      path: failure.log
```

download and print error data

```yaml
name: Time artifact
on:
  push:
jobs:
  create: ...
    
  print:
    runs-on: ubuntu-latest
    needs: create # check that create step is completed
    steps:
      - name: Download the time artifact
        uses: actions/download-artifact@v3
        with:
          name: time-artifact
          path: ~/time.txt # /home/runner/time.txt

      - name: Print the contents of the text file
        run: cat time.txt
```

#### path

 - `./*` all files and folders
 - `!./*.yml` exclude .yml files

```yaml
path: |
  ./* 
  !./.git/** 
  !./*.yml
```


### Cache

GitHub Workflow runs often reuse the same downloaded dependencies from one run to another. For example, package and dependency management tools such as npm and pip keep a local cache of downloaded dependencies.
To cache dependencies for a job, you'll need to use GitHub's cache action. This action retrieves a cache identified by a unique key.

actions themselves have 3 parameters
- `path` **(**required): The file path on the runner to cache or restore. The path can be an absolute path or relative to the working directory.
- `key` **(**required): The key created when saving a cache and the key used to search for a cache. It tells GitHub Actions how to uniquely identify each version of your cache.
- `restore-keys` **(**optional): An ordered list of alternative keys to find the cache if no cache hit occurred for `key`.

### Cache vs Atrifacts

| #   | Key difference       | Artifacts                                                                                     | Cache                                                                                                                    |
| --- | -------------------- | --------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| 1   | **Purpose**          | Sharing data between jobs — for instance, build and deployment jobs.                          | Speed up workflow execution by reusing files that don't change regularly.                                                |
| 2   | **Common files**     | Build and test outputs: Compressed files, binary or executable files, test reports, and logs. | Dependencies: files generated by package managers such as npm, yarn, or pip. For example: _node_modules_.                |
| 3   | **Sharing**          | Artifacts can only be shared between jobs in the same workflow.                               | Cached files are shared between jobs or workflows.                                                                       |
| 4   | **Retention period** | 90 days in public repositories and up to 400 days in private repositories.                    | Cached files that have not been accessed in 7 days are automatically removed. Up to 10 GB of cache files per repository. |

## Workflow syntax

`.github/workflows`

```yaml
# your-repo-name/.github/workflows/first_workflow.yml

name: First Workflow    # name of the workflow
                                         
on: push   # on - specifies when the workflow will be executed
                                               
jobs:
                         
  first-job:                           
    name: Name of first step                    
    runs-on: ubuntu-latest # specifies the virtual environment
    steps:

      #step 1                           
      - name: Print a greeting  # Each step is preceded by `-`
        run: echo Hi from our first workflow! # All the steps are executed sequentially.

      #step 2 
      - uses: actions/checkout@v2.3.4    # v 2.3.4 of the checkout package should be used
  second-job:
    strategy:
      matrix:
        runtimes: [10, 12, 14]
        os_version: [ubuntu-latest, windows-latest]
    runs-on: ${{ matrix.os_version}}
    steps:
      - uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.version }}
```

## Actions

- `uses` : This value takes the format of `action-name/version` or `github-repo-owner/repo-name`.
- `with` : The value is an input if the action requires one.

ways to reference a public action from the marketplace.
1. Referencing a branch
2. Referencing a version
3. Referencing a commit

```yaml
steps:
 - name: step-name
   uses: user/repo-name@branch/version/commit-name
   with: #inputs
     fetch-depth: 0
```

### Events that trigger workflows

what events trigger your workflow

list of events - [link](https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows)

syntax:

```yaml
on:
  [event]: 
    [activity-type]
```

examples:

```yaml
on: [push, fork]
```

```yaml
on:
  push:
    branches:
      - main
      - 'releases/**'
```

```yaml
on:
  push:
    branches: [main]
  pull_request:
    types: [opened, reopened, synchronize]
```

`workflow_dispatch` - manual trigger for workflow

```yaml
name: Manual trigger

on:
  workflow_dispatch:
    inputs:
      name:
        description: "Whom to greet"
        default: "World"

jobs:
  hello:
    runs-on: ubuntu-latest
    steps:
      - name: Hello Step 
        run: echo "Hello ${{ github.event.inputs.name }}" 
```

`workflow_run` - based on other workflow statuses

```yaml
name: Test

on:
  workflow_run:
    workflows: [Workflow Name]
    types: [completed]

jobs:
  on-success:
    runs-on: ubuntu-latest
    if: ${{ github.event.workflow_run.conclusion == 'success' }}
    steps:
      - name: Print Success Message
        run: echo 'The triggering workflow passed'

  on-failure:
    runs-on: ubuntu-latest
    if: ${{ github.event.workflow_run.conclusion == 'failure' }}
    steps:
      - name: Print Failure Message
        run: echo 'The triggering workflow failed'
```

`workflow_call` - allows invoke and reuse another workflow within the same repository or across repositories

```yaml
name: Workflow A (Ping)
on:
  push:
    branches:
      - main

jobs:
  ping:
    uses: USER_OR_ORG_NAME/REPO_NAME/.github/workflows/pong.yaml@master
    with:
      ping: 'PONG'
```

```yaml
name: Workflow B (Pong)
on:
  workflow_call:
    inputs:
      ping:
        required: true
        type: string

jobs:
  pong:
    runs-on: ubuntu-latest
    steps:
      - name: Pong
        run: echo "${{ inputs.ping }}"
```

`repository_dispatch` - generic event triggered by custom events you define

```yaml
name: Remote Dispatch Responder (Repository A)

on: repository_dispatch

jobs:
  ping-pong:
    runs-on: ubuntu-latest
    steps:
      - name: Event Information
        run: |
          echo "Event '${{ github.event.action }}' received from '${{ github.event.client_payload.repository }}'"
```

```yaml
name: Remote Dispatch Action (Repository B)

on:
  push:

jobs:
  ping-pong:
    runs-on: ubuntu-latest
    steps:
      - name: PING 
        run: |
          curl -L \
            -X POST \
            -H "Accept: application/vnd.github+json" \
            -H "Authorization: token ${{secrets.ACCESS_TOKEN}}"\
            -H "X-GitHub-Api-Version: 2022-11-28" \
            https://api.github.com/repos/OWNER/REPO/dispatches \
            -d '{"event_type": "ping", "client_payload": { "repository": "'"$GITHUB_REPOSITORY"'" }}'
```

## Environment variable

### Scopes

1. Workflow level: available to all jobs and all steps within those jobs
2. Job level: can only be accessed by all steps within the specific job
3. Step level: can only be accessed by a specific step

### Access types

1. Interpolation
   syntax `${{ ... }}`
   example `echo ${{ env.VARIABLE }}` 
2. Variable reference
   syntax `$VARNAME`
   example `echo $VARIABLE`

```yaml
name: environment_variable_tutorial
on: [push] 
env:
  # This custom environment variable is available to all the jobs and steps
  TOPIC: Envs and secrets   
jobs:
  job1:
    env:
      # This custom environment variable is available for all the steps
      section: Dev tools   
    runs-on: ubuntu-latest 
    steps:
      - name: step 1
        env:
          # This custom environment variable is available to only this specific step
          platform: the platform
        # Referencing the environment variables
        run: echo "Welcome to the ${{ env.TOPIC }} topic from $section section at $platform"
```

### Default environment variable

[list](https://docs.github.com/en/actions/learn-github-actions/variables#default-environment-variables) 

good practice to incorporate environment variables when using actions to access the file system rather than using hard-coded file paths

| Def. env var      | Description                                                  |
| ----------------- | ------------------------------------------------------------ |
| GITHUB_ ACTOR     | Returns GitHub username or app name that started the workflow|
| GITHUB_REPOSITORY | Returns name of the repository and owner                     |
| GITHUB_REF        | will return Branch or tag ref that triggered the workflow    |

```yaml
name: env_tutorial
on: [push] 

jobs:
  job1:
    runs-on: macos-latest 
    steps:
      - name: step 1
        run: |
          echo "The job ID is $GITHUB_JOB"   
          echo "The name of the event triggered by the workflow is $GITHUB_EVENT_NAME"  
          echo "The name of the workflow is $GITHUB_WORKFLOW" 
          echo "The runner of the workflow is $RUNNER_OS"
```

### Secrets

1. Secret names must contain alphanumeric characters ([a-z], [A-Z], [0-9]).
2. Secret names cannot contain space. Underscores (_) are allowed.
3. Secret names cannot begin with a number.
4. Secret names are not case-sensitive.

Secret Types:

1. Environment
2. Repository
3. Organizational-level

examples:

```yaml
name: secret_workflow_repository

on: [push]

jobs:
  job1:
    runs-on: ubuntu-latest
    steps:
      - name: Step with a secret
        run: |
          echo "Hello World"
          echo "This is a secret value: ${{ secrets.SECRET_VALUE }}"
```

```yaml
name: secret_workflow_environment

on: [push]

jobs:
  job1:
    environment: production_secret
    runs-on: ubuntu-latest
    steps:
      - name: Run Commands with a Secret
        run: |
          echo "Hello World!"
          echo "This is a secret value: ${{ secrets.SECRET_VAL }}"
```
