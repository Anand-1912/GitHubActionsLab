## About Reusable Workflows

[REF](https://resources.github.com/learn/pathways/automation/intermediate/create-reusable-workflows-in-github-actions/)

- Reusable workflows allow you to embrace the DRY (Don’t Repeat Yourself) principle

- A reusable workflow is a GitHub Actions file that can be executed by other workflows. A single workflow can call multiple reusable workflows, with each reference taking up just one line of YAML. This means that the “caller” workflow may contain just a few lines of YAML, but perform a large number of tasks, since it runs each “called” workflow entirely, as if its jobs and steps were part of its own. Additionally, workflows can be [nested](https://docs.github.com/en/actions/using-workflows/reusing-workflows#nesting-reusable-workflows) up to a maximum of four levels.

- We can pass the **Inputs and Secrets** to the Called Workflow (i.e) Reusable Workflow from the Caller Workflow

```yml
on:  # Specifies the event triggering the workflow
  workflow_call:  # Indicates that this is a reusable workflow
    inputs:  # Defines the inputs that can be passed from the caller workflow
      config-path:  # Name of the input
        required: true  # Specifies that this input is mandatory
        type: string  # Specifies the type of the input
    secrets:  # Defines the secrets that can be passed from the caller workflow
      envPAT:  # Name of the secret
        required: true  # Specifies that this secret is mandatory
```

To pass named inputs to a called workflow, use the with keyword in a job. Use the secrets keyword to pass named secrets. For inputs, the data type of the input value must match the type specified in the called workflow (either boolean, number, or string).

```yml
jobs:
  call-workflow-passing-data:
    uses: octo-org/example-repo/.github/workflows/reusable-workflow.yml@main
    with:
      config-path: .github/labeler.yml
    secrets:
      envPAT: ${{ secrets.envPAT }}
```


## Important Info
-------------

1) If you reuse a workflow from a different repository, any actions in the called workflow run as if they were part of the caller workflow. 

 - For example, if the called workflow uses **actions/checkout**, the action checks out the contents of the repository that hosts the **caller workflow**, not the called workflow.

 2) When a reusable workflow is triggered by a caller workflow, the **github context is always associated with the caller workflow**.

 
 3) Actions and Workflows defined in a Private repo of an Organization can be accessed by other Private and Public repos in the Organization if it is enabled (ar the called workflow repos).

 4) If the caller is private, it can access workflows defined in both Public and private(if allowed) workflows.

    If the caller is public, it can access only public repo's workflows.

 5) The GITHUB_TOKEN permissions passed from the caller workflow can be only downgraded (not elevated) by the called workflow. 

### Limitations

1) You can call a maximum of **20 unique reusable workflows** from a single workflow file.

2) Max 4 Nested Workflow levels - including the top level.

   *caller-workflow.yml* → **called-workflow-1.yml** → **called-workflow-2.yml** → **called-workflow-3.yml**

3) *env* context is not shared between **caller to called and called to caller**.


```yml

jobs:
  call-workflow-passing-data:
    uses: octo-org/example-repo/.github/workflows/reusable-workflow.yml@main
    with:
      config-path: .github/labeler.yml
    secrets:
      envPAT: ${{ secrets.envPAT }}

```

```yml

# Workflows that call reusable workflows in the same organization or enterprise can use the inherit keyword to implicitly pass the secrets.
jobs:
  call-workflow-passing-data:
    uses: octo-org/example-repo/.github/workflows/reusable-workflow.yml@main
    with:
      config-path: .github/labeler.yml
    secrets: inherit

```



```yml

# Calling a reusable workflow in matrix definition

jobs:
  ReuseableMatrixJobForDeployment:
    strategy:
      matrix:
        target: [dev, stage, prod]
    uses: octocat/octo-repo/.github/workflows/deployment.yml@main
    with:
      target: ${{ matrix.target }}
```




