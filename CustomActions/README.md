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
  main: "dist/index.js"

 ``` 

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