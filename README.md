# workflows

This repository contains...

- Introduce
    - What is github action?
    - what is workflow?
- Syntax
    - What kind of documents do I include?
- Example
- References
    - sdras/awesome-actions
    - stevemao/github-issue-templates

<hr>

## Introduce

### ✅ What is GitHub action?

GitHub Action is literally Automatical Services, running with workflwos.

It's some kind of software, has the following characteristics.

1. Automate
2. Customate
3. Execute

### ✅ What is workflow?

`workflows` is Automatical Service, supported by GitHub Action and etc...

<hr>

## Syntax

### ✅ What kind of documents do I include?

| Sort | Title | Description |
| :--- | :---- | :---------- |
| Base | [name.md]() <br> [on.md]() | How can I specify some `**/workflow.yaml`. <br> How can I specify when execute `**/workflow.yaml`. |
| Advance | [env.md]() <br> [default.md]() <br> [permission.md]() <br> [concurrency.md]() | How can I use `map` to use values, with security. <br> How can I set default course, into `jobs` or `steps`. <br> How can I grant permission, into `repo`, `workflow`, or `jobs`. <br> How can I grant concurrency, into `workflow` or `jobs`. |
| Jobs | [jobs.md]() | How can I execute `Shared Job` or `Customized Job`. |
| Filter | [filter_pattern.md]() | How can I specify `files` and `directory`, using RegExp Syntax. |
| Context | [context.md]() | How can I use `map` into anywhere. |

<hr>

## Exmaple

| file | purpose | permission | Docs |
| :--- | :------ | :--------- | :----- |
| `greeting.by.bot.yaml` | Auto Greeting with Bot, when you create `issue`, `pull-request` | issue: write <br> pull-request: write | [> Click]() |
| `labeling.by.bot.yaml` | Auto Labeling with Bot, when you create `pull-request` | pull-request: write | [> Click]() |

<hr>

## References

### ✅ sdras/awesome-actions

> A curated list of awesome things related to GitHub Actions.
> More... [> Click](https://github.com/sdras/awesome-actions)

Actions are triggered by GitHub platform events directly in a repo and run on-demand workflows either on Linux, Windows or macOS virtual machines or inside a container in response. With GitHub Actions you can automate your workflow from idea to production.

### ✅ stevemao/github-issue-templates

> A collection of GitHub `issue` and `pull-request` templates.
> More... [> Click](https://github.com/stevemao/github-issue-templates)

### ✅ devspace/awesome-github-templates

> This is a curated list of templates that can offer inspiration for your project. An awesome template is one that informs contributors how to proceed in a very detailed or unique way.
> More... [> Click](https://github.com/devspace/awesome-github-templates)