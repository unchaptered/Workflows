# Default

You can set `Default Process` into all workflows.

> More [> Click](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#defaults)

## defaults.run

To set
- shell
- working-directory

```yaml
defaults:
    run:
        shell: bash
        working-directory: scripts
```

## jobs.<job_id\>.defaults

> More [> Click](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_iddefaults)