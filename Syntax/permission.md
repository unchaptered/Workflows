# Permission

To `modify` the default permission of `GITHUB_TOKEN`.

> References [> Click](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#permissions)

## Access-Free

If you want `GITHUB_TOKEN` has all permission of your repo.

```yaml
permissions: read-all|write-all
```

Or you grant GITHUB_TOKEN to access partially into `read-all` or `write-all`,

```yaml
permission: read-all
```


## Access-Ban

If you want `GITHUB_TOKEN` han no permission of your repo.


```yaml
permissions: {}
```

## Options

Maybe, most `key-value` is updated key-value.

```yaml
permissions:
  actions: read|write|none
  checks: read|write|none
  contents: read|write|none
  deployments: read|write|none
  id-token: read|write|none
  issues: read|write|none
  discussions: read|write|none
  packages: read|write|none
  pages: read|write|none
  pull-requests: read|write|none
  repository-projects: read|write|none
  security-events: read|write|none
  statuses: read|write|none
```