## About Reusable Workflows

[REF](https://resources.github.com/learn/pathways/automation/intermediate/create-reusable-workflows-in-github-actions/)

- Reusable workflows allow you to embrace the DRY (Don’t Repeat Yourself) principle

- A reusable workflow is a GitHub Actions file that can be executed by other workflows. A single workflow can call multiple reusable workflows, with each reference taking up just one line of YAML. This means that the “caller” workflow may contain just a few lines of YAML, but perform a large number of tasks, since it runs each “called” workflow entirely, as if its jobs and steps were part of its own. Additionally, workflows can be [nested](https://docs.github.com/en/actions/using-workflows/reusing-workflows#nesting-reusable-workflows) up to a maximum of four levels.

- We can pass the **Inputs and Secrets** to the Called Workflow (i.e) Reusable Workflow from the Caller Workflow

```
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

```
jobs:
  call-workflow-passing-data:
    uses: octo-org/example-repo/.github/workflows/reusable-workflow.yml@main
    with:
      config-path: .github/labeler.yml
    secrets:
      envPAT: ${{ secrets.envPAT }}
```
