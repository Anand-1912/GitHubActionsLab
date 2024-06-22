### Self Hosted Runners

1. A self-hosted runner is a system that you deploy and manage to execute jobs from GitHub Actions on GitHub.com

2. Self-hosted runners **offer more control of hardware, operating system, and software tools than GitHub-hosted runners provide.**

3. Self-hosted runners can be physical, virtual, in a container, on-premises, or in a cloud.


### Scopes

You can add self-hosted runners at various levels in the management hierarchy:

1) *Repository-level* runners are dedicated to a single repository.

2) *Organization-level* runners can process jobs for multiple repositories in an organization.

3) *Enterprise-level *runners can be assigned to multiple organizations in an enterprise account.


- Your runner machine connects to GitHub using the GitHub Actions **self-hosted** runner application

- When a new version of Runner Appliation is released, **the runner application automatically updates itself when a job is assigned to the runner, or within a week of release if the runner hasn't been assigned any jobs.**

- A self-hosted runner is automatically removed from GitHub if it has not connected to GitHub Actions for more than 14 days. 


# Differences between GitHub-hosted and Self-hosted Runners

| Feature                                           | GitHub-hosted Runners                                                                                               | Self-hosted Runners                                                                                                                                                              |
|---------------------------------------------------|---------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Updates**                                       | Automatic updates for OS, preinstalled packages and tools, and runner application                                    | Automatic updates for the runner application only (can be disabled). You are responsible for updating the OS and other software.                                                |
| **Management**                                    | Managed and maintained by GitHub                                                                                    | You are responsible for managing and maintaining the runners.                                                                                                                   |
| **Job Execution**                                 | Provides a clean instance for every job execution                                                                   | Does not require a clean instance for every job execution.                                                                                                                      |
| **Cost**                                          | Uses free minutes on your GitHub plan; per-minute rates apply after surpassing the free minutes                      | Free to use with GitHub Actions, but you are responsible for the cost of maintaining your runner machines.                                                                       |
| **Customization**                                 | Limited to the environment provided by GitHub                                                                       | Highly customizable to your hardware, operating system, software, and security requirements.                                                                                     |
| **Infrastructure**                                | Hosted on GitHub’s infrastructure                                                                                   | Can use cloud services or local machines that you already pay for.                                                                                                              |


------------------------------------


### Limitations with Self Hosted Runners

Here's a summary of the provided information:

- **Job Execution Time:** Each job can run up to 5 days. Jobs exceeding this limit are terminated and fail.
- **Workflow Run Time:** Each workflow run is limited to 35 days, including execution, waiting, and approval time. Exceeding this limit cancels the workflow run.
- **Job Queue Time:** Jobs for self-hosted runners queued for over 24 hours (up to 48 hours) will be canceled if not started.
- **API Requests:** Up to 1,000 GitHub API requests per hour per repository. Exceeding this limit causes additional API calls and jobs to fail.
- **Job Matrix:** A maximum of 256 jobs can be generated per workflow run, applicable to both GitHub-hosted and self-hosted runners.
- **Workflow Run Queue:** No more than 500 workflow runs can be queued within 10 seconds per repository. Exceeding this limit terminates the workflow run.

- **Registering Self-hosted Runners:** A maximum of 10,000 self-hosted runners can be registered in one runner group. Exceeding this limit prevents adding new runners.


### Network requirements

 - The self-hosted runner connects to GitHub to receive job assignments and to download new versions of the runner application. The self-hosted runner uses an HTTPS long poll that opens a connection to GitHub for 50 seconds, and if no response is received

 - The connection between self-hosted runners and GitHub is over HTTPS (port 443).

 - The Machine should have a minimum of 70 kbits/sec upload and download internet speed to connect to the following GitHub hosts.

    - github.com
    - api.github.com
    - *.actions.githubusercontent.com
    - *.blob.core.windows.net
    - objects.githubusercontent.com
    - objects-origin.githubusercontent.com
    - github-releases.githubusercontent.com
    - github-registry-files.githubusercontent.com


### Security

  - GitHub recommends that the Self Hosted Runners are used only with the Private Repositories.

  - This is because forks of a public repository can potentially run dangerous code on self-hosted runner machine by creating a pull request that executes the code in a workflow.

  - This is not an issue with GitHub-hosted runners because each GitHub-hosted runner is always a clean isolated virtual machine, and it is destroyed at the end of the job execution.


### Adding Self Hosted Runners

 - You can add a self-hosted runner to a **repository, an organization, or an enterprise.**

#### Adding Self Hosted Runners to a Repository

 - To add a self-hosted runner to a user repository, you must be the **repository owner**. 

 - Self Hosted Runners can be added using [APIs](https://docs.github.com/en/rest/actions/self-hosted-runners?apiVersion=2022-11-28) and UI as well

 - Not for all Repositories in an Organization, Self Hosted Runners can be added.Organization owners can choose which repositories are allowed to create repository-level self-hosted runners.[Refer Images folder] 
 
 - [REF](https://docs.github.com/en/organizations/managing-organization-settings/disabling-or-limiting-github-actions-for-your-organization#limiting-the-use-of-self-hosted-runners)


 - [REF](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/adding-self-hosted-runners#adding-a-self-hosted-runner-to-a-repository)


#### Autoscaling Self Host Runners

- [workflow_job] --> queued event --> deploy the runner

- [workflow_job] --> completed event --> remove the runner.

### IP Allowed list.

[REF](https://docs.github.com/en/enterprise-cloud@latest/organizations/keeping-your-organization-secure/managing-security-settings-for-your-organization/managing-allowed-ip-addresses-for-your-organization#using-github-actions-with-an-ip-allow-list)

### Proxy Configuration.

[REF](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/using-a-proxy-server-with-self-hosted-runners)

##### Environmental Variables: (.env)

https_proxy
http_proxy
no_proxy

### Labeling Self Hosted Runners

[REF 1](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/using-labels-with-self-hosted-runners)

[REF 2](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/using-self-hosted-runners-in-a-workflow)

------------------------------------------------

**Default Labels for Self Hosted Runner:**

A self-hosted runner automatically receives certain labels when it is added to GitHub Actions. These are used to indicate its operating system and hardware platform:

**self-hosted**: Default label applied to self-hosted runners.
**linux, windows, or macOS**: Applied depending on operating system.
**x64, ARM, or ARM64**: Applied depending on hardware architecture.

routing jobs using default labels => 

- runs-on: [self-hosted, linux, ARM64]

using custom labels =>

- runs-on: [self-hosted, linux, x64, gpu] here *gpu* is the custom label

```sh
./config.sh --url <REPOSITORY_URL> --token <REGISTRATION_TOKEN> --labels gpu

./config.sh --url <REPOSITORY_URL> --token <REGISTRATION_TOKEN> --labels gpu,x64,linux
```

### routing using groups and labels:

```yml

name: learn-github-actions
on: [push]
jobs:
  check-bats-version:
    runs-on:
      group: ubuntu-runners
      labels: ubuntu-20.04-16core
steps:
--
```

### Managing access to self-hosted runners using groups

- You can use policies to limit access to self-hosted runners that have been added to an organization. (using Runner Groups)

- Enterprise accounts, **organizations owned by enterprise accounts, and organizations using GitHub Team can create and manage additional runner groups.**

- When creating a runner group, you must choose a policy that defines which repositories have access to the runner group. To change which repositories and workflows can access the runner group, organization owners can set a policy for the organization.

[Creating Runner Groups for Self Hosted Runners](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/managing-access-to-self-hosted-runners-using-groups#creating-a-self-hosted-runner-group-for-an-organization)


[Monitoring and Troubleshooting Self Hosted Runners](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/monitoring-and-troubleshooting-self-hosted-runners)

### Runner groups are managed through Organization account's settings :

settings -> Actions -> Runners


### Ephemeral Self Hosted Runners

- To add an ephemeral runner to your environment, include the --ephemeral parameter when registering your runner using config.sh. For example:

```sh

./config.sh --url https://github.com/octo-org --token example-token --ephemeral

```
- The GitHub Actions service will then automatically de-register the runner after it has processed one job. You can then create your own automation that wipes the runner after it has been de-registered.

- Note: If a job is labeled for a certain type of runner, but none matching that type are available, the job does not immediately fail at the time of queueing. Instead, the job will remain queued until the 24 hour timeout period expires.

### Ephemeral vs Persistent Self-Hosted Runners in GitHub Actions

| Aspect                        | Ephemeral Self-Hosted Runners                               | Persistent Self-Hosted Runners                             |
|-------------------------------|-------------------------------------------------------------|------------------------------------------------------------|
| **Lifecycle**                 | Created and destroyed for each job                          | Remain active for multiple jobs                             |
| **State**                     | Clean state for every job                                   | Maintains state between jobs                                |
| **Security**                  | More secure, less risk of state contamination               | Higher risk of state leakage or data contamination          |
| **Maintenance**               | Minimal, as they are set up fresh for each job              | Requires regular updates and management                     |
| **Scalability**               | Can dynamically scale up and down based on job demand       | Scalability depends on provided infrastructure              |
| **Setup Time**                | Requires setup time for each job                            | Faster job start times due to reusable environment          |
| **Resource Utilization**      | Optimized for environments with variable workloads          | Efficient for continuous and consistent workloads           |
| **Use Cases**                 | CI/CD pipelines, security-sensitive workflows               | Long-running services, cost management                      |

### Controlling runner software updates on self-hosted runners

- **Automatic Software Updates:**
  - By default, self-hosted runners automatically update when a new runner software version is available.

- **Ephemeral Runner Updates:**
  - Using ephemeral runners in containers can lead to repeated updates when a new version is released.

- **Disabling Automatic Updates:**
  - Turn off automatic updates to control runner version updates on your schedule.

- **How to Disable Updates:**
  - Use the `--disableupdate` flag when registering your runner with `config.sh`. Example command:
    ```sh
    ./config.sh --url https://github.com/YOUR-ORGANIZATION --token EXAMPLE-TOKEN --disableupdate
    ```

- **Manual Updates Required:**
  - If you disable automatic updates, you must regularly update your runner version.

- **Functionality Dependence:**
  - New GitHub Actions features require updates to both the service and runner software.

- **Update Within 30 Days:**
  - You must update your runner within 30 days of a new version being available.

- **Release Notifications:**
  - Subscribe to notifications for releases in the `actions/runner` repository for update alerts.

- **Latest Version Instructions:**
  - Follow installation instructions for the latest runner version.

- **Update Requirements:**
  - All updates (major, minor, patch) are considered necessary. Not updating within 30 days will prevent GitHub Actions from queuing jobs to your runner.

- **Critical Security Updates:**
  - Jobs will not queue to your runner until it’s updated if a critical security update is required.


# Authentication Requirements

- **Registering and Deleting Runners:**
  - You can register and delete repository and organization self-hosted runners using the API.
  - To authenticate to the API, your autoscaling implementation can use an access token or a GitHub App.

- **Access Token Scopes:**
  - For private repositories, use an access token with the `repo` scope.
  - For public repositories, use an access token with the `public_repo` scope.
  - For organizations, use an access token with the `admin:org` scope.

- **GitHub App Permissions:**
  - For repositories, assign the `administration` permission.
  - For organizations, assign the `organization_self_hosted_runners` permission.

- **Enterprise Runners:**
  - You can register and delete enterprise self-hosted runners using the API.
  - To authenticate to the API, your autoscaling implementation can use an access token with the `manage_runners:enterprise` scope.

### Adding a self hosted runners to a runner group when registering the runner:

- You can use the configuration script to automatically add a new runner to a group. For example, this command registers a new runner and uses the --runnergroup parameter to add it to a group named rg-runnergroup.

```sh

./config.sh --url $org_or_enterprise_url --token $token --runnergroup rg-runnergroup

```

The command will fail if the runner group doesn't exist:

- Could not find any self-hosted runner group named "rg-runnergroup". 

### Monitoring and troubleshooting the Self hosted runners

- Network connectivity
  
   - you can use the self-hosted runner application's **config script** with the **--check parameter** to check that a self-hosted runner can access all required network services on GitHub.com.

In addition to --check, you must provide two arguments to the script:

**--url** with the URL to your GitHub repository, organization, or enterprise. For example, --url https://github.com/octo-org/octo-repo.

**--pat** with the value of a personal access token (classic), which must have the **workflow** scope, or a fine-grained personal access token with workflows read and write access . For example, --pat ghp_abcd1234. For more information, see "Managing your personal access tokens."

### Disabling TLS certificate verification

By default, the self-hosted runner application verifies the TLS certificate for GitHub. If you encounter network problems, you may wish to disable TLS certificate verification for testing purposes.

To disable TLS certification verification in the self-hosted runner application, set the **GITHUB_ACTIONS_RUNNER_TLS_NO_VERIFY** environment variable to 1 before configuring and running the self-hosted runner application.

### Reviewing Log Files

- Runner Logs 

You can monitor the status of the self-hosted runner application and its activities. Log files are kept in the **_diag** directory where you installed the runner application (C:\actions-runner), and a new log is generated each time the application is started. The filename begins with Runner_, and is followed by a UTC timestamp of when the application was started.

Warning:  Runner application log files for **ephemeral runners** must be forwarded and preserved externally for troubleshooting and diagnostic purposes. 


- Job Logs

The self-hosted runner application creates a detailed log file for each job that it processes. These files are stored in the _diag directory where you installed the runner application, and the filename begins with Worker_.

Runner Application's self update logs can be found in SelfUpdate folder inside the _diag folder and it can be present in Runner_ log files.


- journalctl and systemctl => can be used to monitor the runner service.

  runner service name template => actions.runner.<org>-<repo>.<runnerName>.service (runner applications installed as service)
