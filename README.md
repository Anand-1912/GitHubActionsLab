GitHub Action Lab

[![Path Filters Workflow](https://github.com/Anand-1912/GitHubActionsLab/actions/workflows/07paths-filter-workflow.yml/badge.svg)](https://github.com/Anand-1912/GitHubActionsLab/actions/workflows/07paths-filter-workflow.yml)

[![Matrix Job - Fail Fast Workflow](https://github.com/Anand-1912/GitHubActionsLab/actions/workflows/32matrix_job_fail_fast.yml/badge.svg)](https://github.com/Anand-1912/GitHubActionsLab/actions/workflows/32matrix_job_fail_fast.yml)
---

## References

> [!IMPORTANT]
> [GH Certified](https://ghcertified.com/study_resources/)

> [!IMPORTANT]
> [Docs Home](https://docs.github.com/en/actions)

> [!IMPORTANT] 
> [Workflow Syntax](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#name)

> [!IMPORTANT] 
> [Filter Pattern Cheat Sheet](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#filter-pattern-cheat-sheet)

> [!IMPORTANT] 
> [Events](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows)

> [!IMPORTANT]
> [metadata syntax](https://docs.github.com/en/actions/creating-actions/metadata-syntax-for-github-actions#runs-for-javascript-actions)

> [!IMPORTANT]
> [Matrix Strategy](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idstrategymatrix)

```
jobs:
  example_matrix:
    strategy:
      matrix:
        version: [10, 12, 14]
        os: [ubuntu-latest, windows-latest]

```
 - Matrix Strategy is defined at the job level.

 - A job will run for each possible combination of the variables. In this example, the workflow will run six jobs, one for each combination of the os and version variables.

 - A matrix will generate a maximum of **256** jobs per workflow run.This limit applies to both GitHub-hosted and self-hosted runners.

 - The order of the variables in the matrix determines the order in which the jobs are created

   1. {version: 10, os: ubuntu-latest}
   2. {version: 10, os: windows-latest}
   3. {version: 12, os: ubuntu-latest}
   4. {version: 12, os: windows-latest}
   5. {version: 14, os: ubuntu-latest}
   6. {version: 14, os: windows-latest}

```
Example using continue-on-error and strategy.fail-fast
------
runs-on: ${{ matrix.os }}
continue-on-error: ${{ matrix.experimental }}
strategy:
  fail-fast: false // Does not fail the matrix if one or more jobs fails and allows the other jobs to execute 
  matrix:
    node: [13, 14]
    os: [macos-latest, ubuntu-latest]
    experimental: [false]
    include:
      - node: 15
        os: ubuntu-latest
        experimental: true
```
 
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

[CodeQL-example](https://github.com/actions/javascript-action/blob/main/.github/workflows/codeql-analysis.yml)

[Status Badges](https://docs.github.com/en/actions/monitoring-and-troubleshooting-workflows/adding-a-workflow-status-badge)

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



Study Guide.
----

1) [Single Event](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#using-a-single-event)

2) [Multiple Events](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#using-multiple-events)


3) Schedule Events
 
 - [Schedule Event - Workflow Syntax](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#onschedule)

 - Schedule Event

  >[!IMPORTANT]
  >[Schedule Event](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#schedule)

 - Cron Expression

    ┌───────────── minute (0 - 59)
    │ ┌───────────── hour (0 - 23)
    │ │ ┌───────────── day of the month (1 - 31)
    │ │ │ ┌───────────── month (1 - 12 or JAN-DEC)
    │ │ │ │ ┌───────────── day of the week (0 - 6 or SUN-SAT)
    │ │ │ │ │
    │ │ │ │ │
    │ │ │ │ │
    * * * * *

4) Manual Events

   - [Repository Dispatch](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#repository_dispatch)

   - [Workflow Dispatch](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#workflow_dispatch)

5) Using Secrets in Github Actions

   - [Secrets](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions)

6)  Workflow Commands

    - [Workflow Commands](https://docs.github.com/en/actions/using-workflows/workflow-commands-for-github-actions#about-workflow-commands)
     
7) Default Environmental Variables
   
    - [Variables](https://docs.github.com/en/actions/learn-github-actions/variables)

8) Github Actions Authentication

   [secret GITHUB_TOKEN](https://docs.github.com/en/actions/security-guides/automatic-token-authentication#about-the-github_token-secret)

9) Adding Scripts to the workflow
   [Scripts](https://docs.github.com/en/actions/learn-github-actions/essential-features-of-github-actions#adding-scripts-to-your-workflow)

10) Passing data between Jobs
    [Artifacts](https://docs.github.com/en/actions/using-workflows/storing-workflow-data-as-artifacts#passing-data-between-jobs-in-a-workflow)

11) [Disabling and enabling a workflow](https://docs.github.com/en/actions/using-workflows/disabling-and-enabling-a-workflow)

```
gh workflow disable <WORKFLOW NAME OR ID OR FILENAME>

gh workflow enable <WORKFLOW NAME OR ID OR FILENAME>
```

11) [Debugging Workflows](https://docs.github.com/en/actions/monitoring-and-troubleshooting-workflows/enabling-debug-logging)

  - refer Debugging folder for screenshots 

```
set the following Repository level configuration varaibles

ACTIONS_RUNNER_DEBUG : true
ACTIONS_STEP_DEBUG : true

```

12) [Downloading Logs from Web UI](https://docs.github.com/en/actions/monitoring-and-troubleshooting-workflows/using-workflow-run-logs#downloading-logs)

13) [Deleting Logs](https://docs.github.com/en/actions/monitoring-and-troubleshooting-workflows/using-workflow-run-logs#downloading-logs)

14) [ Downloading logs using API](https://docs.github.com/en/rest/actions/workflow-runs?apiVersion=2022-11-28#download-workflow-run-logs)

15) [starter workflows](https://docs.github.com/en/actions/using-workflows/creating-starter-workflows-for-your-organization)

16) [Caching](https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows#about-caching-workflow-dependencies)

17) [Creating Actions](https://docs.github.com/en/actions/creating-actions)

