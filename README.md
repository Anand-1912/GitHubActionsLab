GitHub Action Lab

[![Path Filters Workflow](https://github.com/Anand-1912/GitHubActionsLab/actions/workflows/07paths-filter-workflow.yml/badge.svg)](https://github.com/Anand-1912/GitHubActionsLab/actions/workflows/07paths-filter-workflow.yml)
---

## References

> [!IMPORTANT]
> [Docs Home](https://docs.github.com/en/actions)

> [!IMPORTANT] 
> [Workflow Syntax](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#name)

> [!IMPORTANT] 
> [Filter Pattern Cheat Sheet](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#filter-pattern-cheat-sheet)

> [!IMPORTANT] 
> [Events](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows)

> [!IMPORTANT]
> [Using - Actions](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idstepsuses)

```
Example: 

Using a public action

{owner}/{repo}@{ref}

You can specify a branch, ref, or SHA in a public GitHub repository.

jobs:
  my_first_job:
    steps:
      - name: My first step
        # Uses the default branch of a public repository
        uses: actions/heroku@main
      - name: My second step
        # Uses a specific version tag of a public repository
        uses: actions/aws@v2.0.1
        
```

**Note**.

 - An action can be used from Public Repository, Private Repository, Docker Hub and other container registries like Github Container Registry.


[Skip Workflow Runs](https://docs.github.com/en/actions/managing-workflow-runs/skipping-workflow-runs)

- Skip instructions only apply to the push and pull_request events.

```Commit Messages
[skip ci]
[ci skip]
[no ci]
[skip actions]
[actions skip]

```

[Working-Directory](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#defaultsrun)

[Expressions](https://docs.github.com/en/actions/learn-github-actions/expressions#about-expressions)

[Contexts](https://docs.github.com/en/actions/learn-github-actions/contexts)

[Matrix](https://docs.github.com/en/actions/using-jobs/using-a-matrix-for-your-jobs)

### Other References

[Bash/Linux Command Gist](https://gist.github.com/bradtraversy/cc180de0edee05075a6139e42d5f28ce?permalink_comment_id=4302581)

### Learn Module Guides

[Essentials] (https://resources.github.com/learn/pathways/automation/essentials/automated-application-deployment-with-github-actions-and-pages/)

### Highlights!

1. You can even trigger workflows from outside of GitHub via an API call and the _repository-dispatch_ event.

[repository-dispatch](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#repository_dispatch)

2. A runner is a machine that executes the jobs in a workflow.

   There are two basic types of runners available: **GitHub-hosted runners and self-hosted runners**.

   A _self-hosted_ runner is a system that you deploy and manage on your own infrastructure to execute GitHub Actions jobs.

   A _GitHub-hosted_ runner provides a freshly-provisioned _virtual machine_ running **Ubuntu Linux, Microsoft Windows, or macOS**. When selecting a GitHub-hosted runner, you can identify the specific **OS version**, or specify **-latest** to use the most recent, stable image available.

   GitHub also offers larger runner versions that have additional features, such as _runner groups_ for access control across an organization and more.

### About Runners

- Actions provides two execution environments for your workflows: **GitHub-hosted runners and self-hosted runners**.

- GitHub-hosted runners are recommended for a vast majority of scenarios. Each GitHub-hosted runner is a new virtual machine containing the runner application, other preinstalled tools, and either Ubuntu Linux, Windows, or macOS.

- With GitHub-hosted runners, machine maintenance and upgrades are taken care of for you, as is the operation of the runner.

- Self-hosted runners, on the other hand, run on servers that you provision and maintain, but they offer a key feature as well: the ability to run workflows on custom hardware for testing.

- Self-hosted runners offer more control of hardware, operating system, and software tools than GitHub-hosted runners provide. With self-hosted runners, you can create custom hardware configurations that meet your needs with processing power or memory to run larger jobs, install software available on your local network, and choose an operating system not offered by GitHub-hosted runners. Self-hosted runners can be physical, virtual, in a container, on-premises, or in a cloud.

### About Environments

- Beyond offering a clearly defined deployment target, environments also provide a variety of settings to help safeguard them. For example, you can restrict **which branches or tags can be deployed to an environment, store environment-specific variables and secrets, and enforce protection rules, such as requiring specific people or teams to review workflow runs before deployment**.

### About Permissions

- In Actions, the **permissions** keyword controls **what tasks or processes our workflow can perform**. Setting the right permissions ensures that our workflow can interact with various parts of our GitHub repository without any issues.

- For example, if our workflow deploys a static app to Github Pages, then we have to adjust the permissions for the **GitHub token**. This token acts like a key, allowing our workflow to access and modify parts of our repository. Since we're deploying to Pages, it's essential that our token has the necessary “write” access.

[Job Permissions](https://docs.github.com/en/actions/using-jobs/assigning-permissions-to-jobs)

- [How ODIC tokens are used in deploy pages actions](https://github.com/actions/deploy-pages/issues/329)

### About Runner Groups

- Enterprise and organization owners can use runner groups to control access to self-hosted runners and larger GitHub-hosted runners at the organization and/or enterprise levels. Runner groups collect sets of runners and create a security boundary around them, allowing you to decide which organizations or repositories are permitted to run jobs on those sets of machines. Policies then control which repositories in an organization have access to which runner group(s).

### Job Summaries

- [Reference 1](https://github.blog/2022-05-09-supercharging-github-actions-with-job-summaries/)

- [Reference 2](https://docs.github.com/en/actions/using-workflows/workflow-commands-for-github-actions?tool=bash#adding-a-job-summary)

## To Do:

1. Check differences between the Default Environment Variables and Context Variables
