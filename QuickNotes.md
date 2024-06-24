1) ## Single event

``` yml
  on: workflow_dispatch
```

2) ## Multiple events

  ``` yml
  on: [ push, workflow_dispatch ]
  ```

3) ## Activity types

  ``` yml
  on: 
    issues:
     types:
        - opened
        - labeled
  ```
  If you specify multiple activity types, only one of those event activity types needs to occur to trigger your workflow. 
  
  If multiple triggering event activity types for your workflow occur at the same time, multiple workflow runs will be triggered. 
  
  For example, the above workflow triggers when an issue is opened or labeled. 
  
  If an issue with **two labels is opened**, three workflow runs will start: one for the issue opened event and two for the two issue labeled events.

  4) ## Filters

  ``` yml
    on: 
        push:
           branches:
            - 'main'
            - 'release/*'
            - 'task**'
            - '!poc**'

    on: 
        push:
           branches-ignore:
            - 'poc**'


  ```

  5) ## Glob Patterns

    | *  |  0 or more characters but not /     | 

    | ** | 0 or more characters                |

    | ?  | 0 or 1 of the preceding character   |

    | +  |1 or more of the preceding characters|
  
   [0-9a-z]+ 

   6) We cannot use **branches** and **branches_ignore** (and Paths - Paths Ignore)  filters for the same event in a workflow

   ```
   You cannot use branches and branches-ignore to filter the same event in a single workflow. If you want to both include and exclude branch patterns for a single event, use the branches filter along with the ! character to indicate which branches should be excluded.
   ```

 7) Schedule Event

    ```yml

    on:
      schedule:
        - cron: `0 5,10 * 12 1`
        - cron: `0 5,15 * 1 2`
        - cron: `20/15 * * * *`

        #20/15 * * * * runs every 15 minutes starting from minute 20 through 59 (minutes 20, 35, and 50).


    ```

8) Manual Events

  ```yml

  # allowed input types => boolean, choice, number, environment or string
   on:
    workflow_dispatch:
     inputs:
       deployment_target:
         type: environment
         required: true
    jobs:
        deploy:
            environment: ${{ inputs.deployment_target }}   
            steps:
             - name: deploy
               ---
  ```
  
  ``` yml

    # POST: https://api.github.com/repos/Anand-1912/GitHubActionsLab/dispatches
    # Accept: application/vnd.github+json
    # Authorization: Bearer <<PAT_CLASSIC_SCOPED_TO_REPO>>
    # Payload 1 : {"event_type":"say-hello","client_payload":{"name":"Anand"}}
    # Payload 2 : {"event_type":"say-hello"}
    # Response: 204 NO CONTENT

  on:
  repository_dispatch:
    types: [test_result]

  jobs:
   run_if_failure:
    if: ${{ !github.event.client_payload.passed }}
    runs-on: ubuntu-latest
    steps:
      - env:
          MESSAGE: ${{ github.event.client_payload.message }}
        run: echo $MESSAGE

  ```

9) *check_run* activity types => created, rerequested, completed and requested_action

10) *check_suite* activity types => completed

11) deployment, deployment_status => no activity types

12) Status check functions => always, success, failure, cancelled

11) workflow commands

    ```
    echo "MY_ENV_VARIABLE=somevalue" >> "$GITHUB_ENV"
    echo "MY_OUT=somevalue" >> "GITHUB_OUTPUT"
    echo "::debug::This is a debug message"

    ```

12) Executing Scripts

```yml

steps: 
  - name: inline-script
    run: echo "Hello!"

  - name: check-out-code
    uses: actions/checkout@v4
  - name: make-the-script-file-executable
    working-directory: Scripts
    run: chmod +x hello.sh  #in unix based runners only
  - name: run-the-script
    working-directory: Scripts #if hello.sh is present in the "Scripts" directory
    run: ./hello.sh

```

13) Publishing Docker Images to 'GHCR'

``` yaml
#
name: Create and publish a Docker image

on:
  push:
    branches: ['release']
env:
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

jobs:
  build-and-push-image:
    runs-on: ubuntu-latest

    permissions:
      contents: read
      packages: write
      attestations: write
      id-token: write
  
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Log in to the Container registry
        uses: docker/login-action@65b78e6e13532edd9afa3aa52ac7964289d1a9c1
        with:
          registry: ${{ env.REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}
    
      - name: Extract metadata (tags, labels) for Docker
        id: meta
        uses: docker/metadata-action@9ec57ed1fcdbf14dcef7dfbe97b2010124a938b7
        with:
          images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}

      - name: Build and push Docker image
        id: push
        uses: docker/build-push-action@f2a1d5e99d037542a71f64918e516c093c6f3fc4
        with:
          context: .
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}

      - name: Generate artifact attestation
        uses: actions/attest-build-provenance@v1
        with:
          subject-name: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME}}
          subject-digest: ${{ steps.push.outputs.digest }}
          push-to-registry: true
```

14) Configuring Service Containers

  - cannot be configured inside Composite Actions

  - If your job runs in a Docker container, you do not need to map ports on the host or the service container. 
  
  - If your job runs directly on the runner machine, you'll need to map any required service container ports to ports on the host runner machine.

  - to get the container port => job.services.<service_label>.ports[<docker_host_post>]

  | Value of ports | Description |
| --- | --- |
| 8080:80 | Maps TCP port 80 in the container to port 8080 on the Docker host. |
| 8080:80/udp | Maps UDP port 80 in the container to port 8080 on the Docker host. |
| 8080/udp | Maps a randomly chosen UDP port in the container to UDP port 8080 on the Docker host. |

 ```yml

 When you specify the Docker host port but not the container port, the container port is randomly assigned to a free port. GitHub sets the assigned container port in the service container context. For example, for a redis service container, if you configured the Docker host port 5432, you can access the corresponding container port using the job.services.redis.ports[5432] context

 jobs:
  build:
    services:
      redis:
        # Docker Hub image
        image: redis
        ports:
          - 5432:5432
        credentials:
          username: ${{ secrets.dockerhub_username }}
          password: ${{ secrets.dockerhub_password }}
      db:
        # Private registry image
        image:  ghcr.io/octocat/testdb:latest
        credentials:
          username: ${{ github.repository_owner }}
          password: ${{ secrets.ghcr_password }}

 ```

 15) Code QL

 ``` yaml

      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Initialize CodeQL
        uses: github/codeql-action/init@v3
        with:
          languages: ${{ matrix.language }}
          config-file: ./.github/codeql/codeql-config.yml

      - name: Autobuild
        uses: github/codeql-action/autobuild@v3

      - name: Perform CodeQL Analysis
        uses: github/codeql-action/analyze@v3

 ```

 16) Caching

 ```yaml
      - name: cache-dependencies
        id: cache-npm
        uses: actions/cache@v3
        with:
          key: npm-deps-${{ hashFiles('**/Caching/package.json') }}
          path: "~/.npm"
 ```

 17) Uploading Artifacts

  - Artifacts allow you to persist data after a job has completed, and share that data with another job in the **same workflow**. An artifact is a file or collection of files produced during a workflow run. 

```yaml

 -  name: 'Upload Artifact'
    uses: actions/upload-artifact@v4
    with:
      name: my-artifact
      path: my_file.txt
      retention-days: 5  #retention days => default 90 days
```

18) Downloading Artifacts

```yaml

  # downloads all the artifacts
  - name: Download all workflow run artifacts
    uses: actions/download-artifact@v4

  # downloads a specific artifact

  - name: Download a single artifact
    uses: actions/download-artifact@v4
    with:
      name: my-artifact

```

19) Running Jobs inside the Container

```yaml
name: CI
on:
  push:
    branches: [ main ]
jobs:
  container-test-job:
    runs-on: ubuntu-latest
    container:
      image: node:18
      env:
        NODE_ENV: development
      ports:
        - 80
      volumes:
        - my_docker_volume:/volume_mount
      options: --cpus 1
    steps:
      - name: Check for dockerenv file
        run: (ls /.dockerenv && echo Found dockerenv) || (echo No dockerenv)
```

20) Concurrency group

   - can be defined at the Job level or at Workflow level.

  ```yaml
  on:
  push:
    branches:
      - main
  concurrency:
    group: ${{ github.workflow }}-${{ github.ref }}
    cancel-in-progress: true
  ```

21) Template Workflows
  
  - **metadata file** => <WORKFLOW_FILE_NAME>.properties.json

  ```json
  {
    "name": "TDK Org DotNet CICD Workflow",
    "description": "TDK Org DotNet starter CICD Workflow",
    "iconName": "netcoresvg",
    "categories": ["Dot Net"],
    "filePatterns": ["^Dockerfile", ".*\\.md$"]
  }

  ```

  ```yml
  name: DotNet Build And Test
  run-name: Workflow triggered by ${{github.actor}}
  on:
    push:
      branches: [$default-branch]
  ```

  22) Actions

   - Docker Container Action - why?

     - Docker containers package the environment with the GitHub Actions code. This creates a more consistent and reliable unit of work because the consumer of the action does not need to worry about the tools or dependencies.

     - A Docker container allows you to use specific versions of an operating system, dependencies, tools, and code. 
     
     - For actions that must run in a **specific environment configuration**, Docker is an ideal option because you can customize the operating system and tools. Because of the latency to build and retrieve the container, **Docker container actions are slower than JavaScript actions**

     - Docker container actions can only execute on runners with a Linux operating system. Self-hosted runners must use a Linux operating system and have Docker installed to run Docker container actions.

  - JS Actions - **actions/toolkit**
  
  - Composite Actions 

    - A **composite action** allows you to combine multiple workflow steps within one action. 
    
    - For example, you can use this feature to bundle together multiple run commands into an action, and then have a workflow that executes the bundled commands as a single step using that action

  - To ensure that your action is compatible with GitHub Enterprise Server, you should make sure that you do not use any hard-coded references to GitHub API URLs. You should instead use environment variables to refer to the GitHub API:

  - For the REST API, use the GITHUB_API_URL environment variable.
  - For GraphQL, use the GITHUB_GRAPHQL_URL environment variable


  - [Release Management of Actions](https://docs.github.com/en/actions/creating-actions/about-custom-actions#using-release-management-for-actions)


  - [Publishing Actions](https://docs.github.com/en/actions/creating-actions/publishing-actions-in-github-marketplace)


  - [managing-access-for-a-private-repository](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/enabling-features-for-your-repository/managing-github-actions-settings-for-a-repository#managing-access-for-a-private-repository)

  - [sharing-actions-and-workflows-with-your-organization](https://docs.github.com/en/actions/creating-actions/sharing-actions-and-workflows-with-your-organization)
