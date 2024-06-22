## Notes

1. Custom Actions can simplify our workflows and they are reusable in multiple workflows.

 - [ref](https://docs.github.com/en/actions/creating-actions/about-custom-actions)

 - [metadata syntax](https://docs.github.com/en/actions/creating-actions/metadata-syntax-for-github-actions#runs-for-javascript-actions)

 - [managing github action settings for a repo](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/enabling-features-for-your-repository/managing-github-actions-settings-for-a-repository)

2. Types of Custom Actions

   - Javascript Actions
   - Docker Actions
   - Composite Actions

**NOTE**

How containers can be used in GitHub Actions?

 - Container Actions (Only Docker Containers)

 - We can run an entire job in the container. (Job Containers)
   - [ref](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idcontainer)

 - we can define *Service containers* for a job.
   - [ref](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idservices)

When we use containers to run a Job or an Action, they have to be executed inside a Linux Machine irrespective of whether we are using Github Hosted runners or Self hosted runners for the runner host.

-----

### Javascript Actions

[JS Actions](https://docs.github.com/en/actions/creating-actions/creating-a-javascript-action)

1) A JS custom action needs an **Entry point JS file** (which contains the action logic) and an Action metadata file which should be named as **Action.yaml** which contains information about the Inputs, Outputs and EntryPoint JS file. 

2) It is recommended to have a separate repository for an Action if it has to be used by publi otherwise it can be created in any path in the repo where it is used. 
  `.github/actions/action-A`

 ```yml
 # metadata file

name: "Hello World"
author: "Anand"
description: "Greet someone and record the time"
inputs:
  who-to-greet: # id of input
    description: "Who to greet"
    required: true
    default: "World"
outputs:
  time: # id of output
    description: "The time we greeted you"
runs:
  using: "node20" #Javascript Runtime
  pre: 'setup.js'
  main: 'index.js'
  post: 'cleanup.js'

 ``` 


 [Metadata Syntax](https://docs.github.com/en/actions/creating-actions/metadata-syntax-for-github-actions)

### Docker Container Actions

[Docker Container Actions](https://docs.github.com/en/actions/creating-actions/creating-a-docker-container-action)

1) A Docker Container needs a Dockerfile (Image) and an Action Metadata file

``` yml

# action.yml
name: 'Hello World'
description: 'Greet someone and record the time'
inputs:
  who-to-greet:  # id of input
    description: 'Who to greet'
    required: true
    default: 'World'
outputs:
  time: # id of output
    description: 'The time we greeted you'
runs:
  using: 'docker'
  image: 'Dockerfile'
  args:
    - ${{ inputs.who-to-greet }}

```


```js
//setting exit code in JS Action

try {
  // something
} catch (error) {
  core.setFailed(error.message);
}

```


```sh
#setting exit code in Docker Container Action

if <condition> ; then
  echo "Game over!"
  exit 1
fi
```

### Publishing Actions to GitHub Market Place - Requirements

- The action must be in a public repository.
- Each repository must contain a single action.
- Each repository must not contain any workflow files.
- The action's metadata file (action.yml or action.yaml) must be in the root directory of the repository.
- The name in the action's metadata file must be unique.
- The name cannot match an existing action name published on GitHub Marketplace.
- The name cannot match a user or organization on GitHub, unless the user or organization owner is publishing the action. For example, only the GitHub organization can publish an action named github.
- The name cannot match an existing GitHub Marketplace category.
- GitHub reserves the names of GitHub features.

### Sharing Actions and Reusable Workflows in an Enterprise

- **Enterprise Sharing:**
  - Share actions and reusable workflows within your enterprise without publishing them publicly.
  - This applies if your organization is owned by an enterprise account.

- **Repository Access:**
  - Allow GitHub Actions workflows to access an internal or private repository containing the action or reusable workflow.

- **Usage Scope:**
  - Actions and reusable workflows in internal or private repositories can be used in other internal or private repositories owned by the same organization or any organization owned by the enterprise.
  - Internal repository actions cannot be used in public repositories.
  - Private repository actions cannot be used in public or internal repositories.

- **Warning:**
  - Making a private repository accessible to workflows in other repositories can indirectly allow outside collaborators to access the private repository by viewing workflow logs.
  - GitHub provides a scoped installation token to the runner with read access to the repository, which expires after one hour.
