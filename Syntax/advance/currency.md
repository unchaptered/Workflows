# concurrency

If you can set Same Currency Group to several Worflows.

- Workflows A
- Workflows B
- Workflows C

These workflows is running, seems like JS Async Await.

If other workflows, sharing group, is running, new workflow will be `pending` to wait End of Other workflows.

```yaml
# base
concurrency: staging_environment

# with env
concurrency: ci.${{github.ref}}
```

<hr>

## Options

- 🤔 cancel in progress
- 🤔 using fallback value
- 🤔 Only cancel-in-progress jobs or runs for the current workflow

> Details [> Click](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#examples-using-concurrency-and-the-default-behavior)

### 🤔 cancel in progress

You can set progress can cansel in progress.

```yaml
concurrency:
    group: ci.${{github.ref}}```
    cancel-in-progress: true
```

### 🤔 using fallback value

If you build the group name with a property _that is only defined for specific events_, you can use a fallback value.

> fallback means '후퇴'

If your workflow response to other events in addition to `pull request` events, you will need to provide a fallback to avoid a syntax error.

The follwing concurrency group cancels in-progress jobs or runs on pull_requests only.

If github.`head_ref` is undefined, the concurrency group will fallback to the run ID, which is guaranteed to be both unique and defined for the run.

```yaml
concurrency:
    group: ${{ github.head_ref || github.run_id}}
    cancel-in-progress: true
```

### 🤔 Only cancel-in-progress jobs or runs for the current workflow

If you have multiple workflows in the same repository, _concurrency group names_ must be unique _across_ workflows to avoid canceling in-progress jobs, _or_, runs from other workflows

To only cance-in-porgress runs of the same workflow, you can use the github.workflow property to build the concurrency group:

```yaml
concurrency:
    group: ${{ github.workflow }} - ${{ github.ref}}
    cancel-in-progress: true
```

