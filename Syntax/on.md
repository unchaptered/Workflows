# on

on is used for **Automatic Trigger** to run `*.yml`.

> References [> Click](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)

| level | target | base | target | link |
| :---- | :--- | :----- | :----- | :---- |
| Base | push / fork | on: | **push:** <br> **fork:** | [> link](https://github.com/unchaptered/Workflows/blob/main/Syntax/on.md#push-or-fork) |
| Base | issue/label | on: | **issue:** <br> **issue_comments:** <br> **label:** | [> link](https://github.com/unchaptered/Workflows/blob/main/Syntax/on.md#issue-and-label) |
| Base | n-events | on: | **push:**/**issue:**/... | [> link](https://github.com/unchaptered/Workflows/blob/main/Syntax/on.md#n-events) |
| Base | schedule | on: | **schedule:** |[> link](https://github.com/unchaptered/Workflows/blob/main/Syntax/on.md#issue-and-label) |
| Filters | push (branches, psuh, tags) | on: <br> push: | **branches:** <br> **branches-ignore:** <br> **tags:** <br> **tags-ignore:** | [> link](https://github.com/unchaptered/Workflows/blob/main/Syntax/on.md#push-with-Filter) |
| Filters | pull-requests (branches) | on: <br> pull-requests: | **branches:** <br> **branches-ignore:** | [> link](https://github.com/unchaptered/Workflows/blob/main/Syntax/on.md#pull-request-with-Filter) |

- All workflows consume github workflows points
    - pull-request : 3 point
    - push into exsisting-branches : 2 point
    - push into new-branches : 2 point

<hr>

## push or fork

```yaml
on: push            # single-event
on: [ push, for ]   # muti-event
```

### push in **Filter**

```yaml
on:
    push:
        branches:
            - main
            - 'releases/**'
```

<hr>

## issue and label

1. `issue_comment` is runned by issue or label
2. `label` is runned by label(craeted-only)
3. `issue` is runned by issue

### issue_comments

```yaml
# usecase 1 / Base
on:
    issue_comments:
        types:
            - created
            - edited
            - deleted
# usecase 2 / provide to use Events-Array
on:
    issue_comments:
        types: [ created, edited, deleted ]
```

<hr>

### label

```yml
on:
    label:
        types:
            - created
```

<hr>

## n-events

```yaml
on:
  label:
    types:
      - created         # label is craeted
  push:
    branches:
      - main            # pushed into `main` branch
  page_build:           # pushed into `gh_pages` branch
```


<hr>

### issue

```yml
on:
    issue_comments:
        types:
            - created
            - edited
            - deleted
```

<hr>

## schedule

more : https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#scheduled-events

```yaml
on:
  schedule:
    - cron: '30 5 * * 1,3'
    - cron: '30 5 * * 2,4'

jobs:
  test_schedule:
    runs-on: ubuntu-latest

    steps:

      - name: Not on Monday or Wednesday
        if: github.event.schedule != '30 5 * * 1,3'
        run: echo "This step will be skipped on Monday and Wednesday"

      - name: Every time
        run: echo "This step will always run"

```

<hr>

## push with **Filter**

- ✅ contain/ignore branches
- ✅ contain/ignore tags
- ✅ contain/ignore paths

### contain/ignore branches

- contain branches
- ignore branches
- advanced wargnings of contain/ignore
    - ignore -> contain = contain
    - contain -> ignore = ignore

```yaml
on:
    push:
        branches:
            - main              # named of 'main'
            - 'mona/octocat'    # named of 'mona/octocat'
            - 'releases/**'     # started by 'releases/'
        branches-ignore:
            - hola              # named of 'hola' ... same
```

### contain/ignore tags

- contain tags
- ignore tags
- advanced wargnings of contain/ignore
    - ignore -> contain = contain
    - contain -> ignore = ignore

```yaml
on:
    push:
        tags:
            - v2            # started by v2
            - v1.*          # started by 'v1.'
                            #  except started by 'v1.test.'
        tags-ignore:
            - v1.test.*     # started by 'v1.test.'
```

### contain/ignore paths

- contain paths
- ignore paths
- advanced wargnings of contain/ignore
    - ignore -> contain = contain
    - contain -> ignore = ignore


```yaml
on:
  push:
    paths:
        - '**.js'
    paths-ignore:
        - 'docs/**'
```

<hr>

## pull-request with **Filter**

- ✅ contain/ignore branches

### contain/ignore branches

- contain branches
- ignore branches
- advanced wargnings of contain/ignore
    - ignore -> contain = contain
    - contain -> ignore = ignore

```yaml
on:
    pull_request:
        branches:
            - main                  # named of 'main'
            - 'mona/octocat'        # named of 'mona/octoact'
            - 'releases/**'         # started from 'relases/'
        branches-ignore:
            - 'unchap/3'            # named of 'unchap/3'
            - 'pre-rel/**'          # started from 'pre-rel/'
```

<hr>

## workflow_call with **Filter**

To use select Params from Workflows

> more [> Click](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#onworkflow_callinputs)

```yaml
on:
  workflow_call:
    inputs:
      username:
        description: 'A username passed from the caller workflow'
        default: 'john-doe'
        required: false
        type: string

jobs:
  print-username:
    runs-on: ubuntu-latest

    steps:
      - name: Print the input name to STDOUT
        run: echo The username is ${{ inputs.username }}
```