# Jobs

A workflow run is made up of one or more `jobs`, which run in parallel by default.

To run jobs sequentially, you can define dependencies on other jobs using the jobs.<job_id\>.needs keyword.

Each job runs in a runner environment specified by `runs-on`.

🤔

You can run an unlimited number of jobs as long as you are within the workflow usage limits.

> More Detail
> - [> Click - Usage limits and billing](https://docs.github.com/en/actions/learn-github-actions/usage-limits-billing-and-administration) for Github-hosted runners
> - [> Click - About-self-hosted runner](https://docs.github.com/en/actions/hosting-your-own-runners/about-self-hosted-runners#usage-limits) for self-hosted runner usage limits.

<hr>

## jobs.<job_id>

1. name
2. permission [Advanced Docs is exsiting> Click](https://github.com/unchaptered/Workflows/blob/main/Syntax/advance/permission.md)
3. needs
    1. needs.if
4. runs-on
5. environment
6. concurrency [Advanced Docs is exsiting> Click](https://github.com/unchaptered/Workflows/blob/main/Syntax/advance/concurrency.md)
7. outputs
8. env [Advanced Docs is exsiting> Click](https://github.com/unchaptered/Workflows/blob/main/Syntax/advance/env.md)
9. defualt [Advanced Docs is exsiting> Click](https://github.com/unchaptered/Workflows/blob/main/Syntax/advance/default.md)
10. steps
11. timeout-minues
12. strategy
13. container @nope
14. services @nope
15. with @not-same steps[*].with

### ✅ name

```yaml
jobs:
    my_first_job:
        name: My first job

    my_second_job:
        name: MY second job
```

### ✅ permission

You can grant permission of `GITHUB_TOKEN`, to allow the minimum access.

To grant permission of token.

- each job in worflows
- all job in wokrflows

If you want to read all properties of permission-value

- link to copied docs [> Click](https://github.com/unchaptered/Workflows/blob/main/Syntax/advance/permission.md#options).
- link to latest docs in official docs [> Click](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idpermissions)

```yaml
# Free Access
permissions: read-all | write-all

# Deny Access
permissions: {}

# Partially Access
permissions:
    actions: read | write | none
    # more than many options ..
```

You can to grant Permission of specific jobs.

```yaml
jobs:
    stale:
        runs-on: ubuntu-latest

        permission:
            issues: write
            pull-requests: write
        
        steps:
            - uses: action/satle@v5
```

### ✅ needs

If you want `Serialized Process` of one process.

You can connect several jobs using needs options.

```yaml
jobs:
    job1:

    job2:
        needs: job1

    job3:
        needs: [ job1, job2]
```

These jobs is serially runned **job1 > job2 > job3** using need option.

#### need.alyways

You can use conditions to `branch out` process.

```yaml
name: example-workflow

on: [ push ]

jobs:
    production-deploy:
        it: github.repository == 'octo-org/octo-repo-prod'

        runs-on: ubuntu-latest

        steps:
            - uses: actions/checkout@v3
            - uses: actions/setup-node@v3
              with:
                node-version: '14'
            - run: npm install -g bats
```

### ✅ runs-on

You can choose `System Type` in Github/Your Hosting Runner.

#### Reccomendation Option 

- ubuntu-latest

#### References

- `-latest` is provided image from Github.
- `-latest` with AWS and Azure etc version can be diff with `-latest(github)`.

> More [> Click](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#choosing-self-hosted-runners)


### ✅ environment

_environment is not same env!_

> link to latest docs in official odcs [> Click](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idenvironment)

Use `job.<job_id>.environment` to define the environment that the job references.

All envrionment protection rules must pass before a job referencing the environment is sent to a runner.

> More Environment [> Click](https://docs.github.com/en/rest/repos#deployments)

🤔

You can provide the environment as only the environment `name`, or as an environment object with the `name` or `url`.

The URL maps to `environment_url` in the deployment API.

> More Deploy [> Click](https://docs.github.com/en/rest/repos#deployments)


```yaml
# base
environment:
    name: production_environment
    url: https://github.com

# using input value
environment:
    name: production_environment
    url: ${{ steps.step_id.outputs.url_output }}
```

### ✅ Concurrency

If you want docs of 'What is concurrency?'.

- link to copied docs [> Click](https://github.com/unchaptered/Workflows/blob/main/Syntax/advance/concurrency.md).
- link to latest docs in official docs [> Click](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#concurrency)

```yaml
# base
concurrency: staging_environment

# using input value
concurrency: ci.${{ github.ref }}

# using concurrency to cancel any in-progress job or run
concurrency:
    group: ${{ github.ref }}
    cancel-in-progress: true

# using fallback value to prevent Syntax Error
concurrency:
    group: ${{ github.ref || github.run_id}}
    cancel-in-progres: true

# only cancel-in-progress jobs or runs for the current workflow
concurrency:
    gorup: ${{ github.workflow }} - ${{ github.ref}}
    cancel-in-progress: true
```

### ✅ Outputs

You can use `jobs.<job_id>.outputs` to create a `map` of outputs for a job.

Job outputs are available to all downsteram jobs that depend on this job.

For more information on defining job dependencies ... [> Click](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idneeds)

🤔

Output are Unicode string.

- 1 file can be a maximum of 1 MB
- 1 workflow can be a maximum of 5 MB

```yaml
jobs:
    job1:
        runs-on: ubuntu-latest

        outputs:
            output1: ${{ steps.output.test }}
            output2: ${{ steps.output.test }}
        
        steps:
            - id: step1
              run: echo "::set-output name=test::hello"
            
            - id: step2
              run: echo "::set-output name=test::world"

    job2:
        runs-on: ubuntu-latest

        needs: job1

        step:
            - run: echo ${{ needs.job1.outputs.output1 }} ${{ needs.job1.outputs.output2 }}
```

### ✅ env

To set environment variables

- all steps in the job
- entire worflow
- individual step

> More [> Click](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idenv)

```yaml
jobs:
    job1:
        env:
            NAME: unchaptered
```

### ✅ defaults

If you want docs of 'What is defaults?'.

- link to copied docs [> Click](https://github.com/unchaptered/Workflows/blob/main/Syntax/advance/default.md).
- link to latest docs in official docs [> Click](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_iddefaults)

```yaml
# job.<job_id>.defualts.run

jobs:
    job1:
        runs-on: ubuntu-latest

        defaults:
            run:
                shell: bash
                working-directory: scripts
```

### 10. steps

You can use run an unlimited number of steps as long as you are within the workflow suage limits.

1. name
2. id
3. if
4. uses
5. run
6. shell
7. with
    1. args
    2. entrypoint
8. env
9. continue-on-error
10. timeout-minutes

```yaml
name: Greeting from Mona

on: push

jobs:
    my-job:
        name: My Job

        runs-on: ubuntu-latest
        
        steps:
            - name: Print a greeting
              env:
                MY_VAR: Hi there! My name is
                FIRST_NAME: Mona
                MIDDLE_NAME: The
                LAST_NAME: Octocat
              run: |
                echo $MY_VAR $FIRST_NAME $MIDDLE_NAME $LAST_NAME
```

#### job.<job_id>.steps[*].name

A name for your step to display on Github

```yaml
steps:
    - name: displayed name on github
```

#### job.<job_id>.steps[*].id

A unique identifier for the step.

You can use the `id` to reference the step in contexts.

- link to copied docs [> Click](https://github.com/unchaptered/Workflows/blob/main/Syntax/context/context.md).
- link to latest docs in official docs [> Click](https://docs.github.com/en/actions/learn-github-actions/contexts#about-contexts)

#### job.<job_id>.steps[*].if

You can use the `if` conditional to prevent a step from running unless a condition is met.

```yaml
# using context
steps:
    - name: My first step
      if: ${{ github.event_name == 'pull_request' && github.event.action == 'unassigned' }}
      run: echo This event is a pull request that had an assignee removed.

# using status check functions
steps:
    - name: My first step
      uses: octo-org/action-name@main
    
    - name: My backup step
      if: ${{ failure() }}
      uses: actions/heroku@1.0.0

# using secrets
name: Run a step if a secret has been set

on: push

jobs:
    my-jobname:
        runs-on: ubuntu-latest

        env:
            super_secret: ${{ secrets.SuperSecret }}
        
        steps:
            - if: ${{ env.super_secret != ''}}
              run: echo 'This step will only run if the secret has a value set'
            
            - if: ${{ env.super_secret == ''}}
              run: echo 'This step will only run if the secret does not have a value set'
```

#### job.<job_id>.steps[*].uses

If you include the version fo the action, specifying...

- Git ref
- SHA
- Docker tag number

You need to exactly type Version of Action.

Bad Sinario, when you don't type Version.

- Using the commit SHA of a released action version is the safest for stability and security.
- Using the specific major action version allows you to receive cirical fixes and security patches while still maintaining compatibility.
    It also assures that your workflow should still work.
- Using the default branh of an action may be convenient, but if someone releases a new major version with a breaking change, your worflow could break.


> More [> Click]()

```yaml
# Using a versioned actions
jobs:
    using_versioned_actions:
        steps:
            - uses: actions/checkout@a81bbbf8298c0fa03ea29cdc473d45769f953675
              # Ref a specific commit

            - uses: actions/checkout@v3
              # Ref the major version of a release

            - uses: actions/checkout@3.2.0
              # Ref a specific version
            
            - uses: acitons/checkout@main
              # Ref a branch

# Using a public action
jobs:
    using_public_actions:
        steps:
            - name: My first Step
              uses: actions/heroku@main
              # Uses the default branch of a public repository

            - name: My second step
              uses: actions/aws@v2.0.1
              # Uses a specific version tag of a public repository

# Using a public action is a subdirectory
jobs:
    using_public_actions_in_sub_directory:
        steps:
            - name: My first Step
              uses: actions/aws/ec2@main

# Using a Docker Hub action

# link > https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#example-using-a-docker-hub-action

# Using the Github Packages Container registry

# link > https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#example-using-the-github-packages-container-registry

# Using a Dokcer public registry action

# link > https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#example-using-a-docker-public-registry-action

# Using an action inside a diffrent private repository than the workflow

jobs:
    my_first_job:
        steps:
            - name: Check out repository
              uses: action/checkout@v3
              with:
                repository: octocat/my-private-repo
                ref: v1.0
                token: ${{ secrets.PERSONAL_ACCESS_TOKEN }}
                path: ./.github/actions/my-private-repo
            
            - name: Run my action
              uses: ./.github/actions/my-private-repo/my-aciton
```

#### job.<job_id>.steps[*].run

```yaml
# A single-line command
- name: Install Dependencies
  run: npm install

# A multi-line command
- name: Clean Install dependencies and build
  run: |
    npm ci
    npm run build

# working-directory
- name: Clean temp directory
  run: rm -rh *
  working-directory: ./temp
```

#### job.<job_id>.steps[*].shell

You can modify `default shell settings`.

> More [> Click](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#example-using-the-github-packages-container-registry)

#### job.<job_id>.steps[*].with

A `map` of the input params defined by the action.

```yaml
jobs:
    job_steps_with_params:
        steps:
            - name: My First Step
              uses: actions/hello_world@main
              with:
                first_name: Mona
                middle_name: The
                last_name: Octocat
```

#### job.<job_id>.steps[*].with.args

A `string` that defines the inptus for a Docker Container

> Mors [> Click](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idstepswithargs)


#### job.<job_id>.steps[*].with.entrypoint

Overrides the Docker `ENTRYPOINT` in the `Dockerfile`, or sets it if one wasn't already specified.

Unlike the Docker `ENTRYPOINT` instructiong which has a shell and exce from, `extrypointr` keyword accepts only a single define any inputs.

#### job.<job_id>.steps[*].env

```yaml
steps:
    - name: My first action
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        FIRST_NAME: Mona
        LAST_NAME: Octocat
```

#### job.<job_id>.steps[*].continue-one-error

If you set this options is true, to allow a job to pass when this step fails.

- Unnecessary process

#### job.<job_id>.steps[*].timeout-minutes

The maximum number of minutes to run the step before killing the process.

### ✅ Timeout-minutes

The `GITHUB_TOKEN` expires when a jo finishes or after a maximum of 24 hours.

For self-hosted runner, the token may be the limiting factor if the job timeout is greater than 24 hours.

> More [> Click](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idtimeout-minutes)

### ✅ strategy

#### job.<job_id>.stategy.matrix

You can use matrix... 

```yaml
jobs:
  example_matrix:
    strategy:
      matrix:
        version: [10, 12, 14]
        os: [ubuntu-latest, windows-latest]

# matrix include : https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idstrategymatrixinclude
# matrix exclude : https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idstrategymatrixexclude
```

```javascript
  { version: 10, os: ubuntu-latest }
  { version: 10, os: windows-latest }
  { version: 12, os: ubuntu-latest }
  { version: 12, os: windows-latest }
  { version: 14, os: ubuntu-latest }
  { version: 14, os: windows-latest }
```

> More [> CLick](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idstrategymatrix)

#### job.<job_id>.strategy.fall-fast

You can handle job-fail...

> Mors [> Click](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idstrategyfail-fast)

#### job.<job_id>.strategy.max-parallel

You can fix max-jobs, parallel...

> More [> Click](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idstrategymax-parallel)

#### job.<job_id>.continue-on-error

<hr>

### ✅ container

> More [> Click](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idcontainer)

### ✅ services

> More [> Click](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idservices)

### ✅ uses

THe location and version of a reusable workflow file to run as a job.

Use one of the following syntaxes...

1. {owner}/{repo}/./github/workflows/{filename}@{ref} for resuable workflows in public
2. ./.github/workflows/{filename} for reusable worflows in same repository.

{ref} can be...

1. SHA
2. release tag
3. branch name

Using the commit SHA is the safest for stability and security.

> [Security hardening for GIthub Action](https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions#reusing-third-party-workflows)

```yaml
jobs:
  call-workflow-1-in-local-repo:
    uses: octo-org/this-repo/.github/workflows/workflow-1.yml@172239021f7ba04fe7327647b213799853a9eb89

  call-workflow-2-in-local-repo:
    uses: ./.github/workflows/workflow-2.yml

  call-workflow-in-another-repo:
    uses: octo-org/another-repo/.github/workflows/workflow.yml@v1
```


### ✅ with

Unlike `jobs.<job_id>.steps[*].with`, the inputs you pass with `jobs.<job_id>.with` are not be available as environment variables in the called workflow.

Instead, you can reference the inputs by using the inputs context.

```yaml
jobs:
  call-workflow:
    uses: octo-org/example-repo/.github/workflows/called-workflow.yml@main
    with:
      username: mona
```

> More [> Click](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idwith)

### ✅ secrets

```yaml
jobs:
  call-workflow:
    uses: octo-org/example-repo/.github/workflows/called-workflow.yml@main
    secrets:
      access-token: ${{ secrets.PERSONAL_ACCESS_TOKEN }}
```


#### job.<job_id>.secrets.inherit

Keyword, `inherit`, to pass all the calling workflow's secrets to the called workflow.

This includes all secrets the calling workflow has access to, namely organization, repository, and envrionment secrets.

The `inherit` keyword cna be used to pass secrets across repositories within the same organization, or across organizations within the same enterprise.

```yaml
# Example 1
on:
  workflow_dispatch:

jobs:
  pass-secrets-to-workflow:
    uses: ./.github/workflows/called-workflow.yml
    secrets: inherit


# Example 2
on:
  workflow_call:

jobs:
  pass-secret-to-action:
    runs-on: ubuntu-latest
    steps:
      - name: Use a repo or org secret from the calling workflow.
        run: echo ${{ secrets.CALLING_WORKFLOW_SECRET }}
```

#### jobs.<job_id>.secrets.<secret_id>

A pair consisting of a string identifier for the secert and the value of the secret.

The identifier must match the anme of a secret defined by `on.workflow_call.secrets.<secret_id>` in the called workflow.

> More on.workflow_call.secrets.<secret_id\> [> Click](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#onworkflow_callsecretssecret_id)