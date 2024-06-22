### GitHub hosted Runners

- GitHub offers hosted virtual machines to run workflows. The VMs contains the tools, packages and settings available for GitHub actions to use.

- Runners are the machines that execute jobs in a workflow. They are available for both *Public and Private Repositories*

- Runners have the Github *Runner Application* installed.

- Available OS: 
   
     - Ubuntu Linux 
     - Windows 
     - MacOS   

- When you use a GitHub-hosted runner, machine maintenance and upgrades are taken care of for you.

### Types of Github Hosted Runners
  
  - Standard Github Hosted Runners

  - Large Runners - available for organizations and enterprises using the **GitHub Team or GitHub Enterprise Cloud plans**.

### Viewing Runners for a Repository

  - needs *repo: write* access. 
  
  - Actions -> Management -> Runners (Self Hosted Runner and GitHub hosted Runners)

> [!IMPORTANT]

> The *-latest* runner images are the latest stable images that GitHub provides, and might not be the most recent version of the operating system available from the operating system vendor.

### Runner's workflow labels

 - ubuntu-latest
 - windows-latest
 - macos-latest

### Standard Github Runners for Public Repo

- The use of these runners on public repositories is free and unlimited.

### Standard Github Runners for Private Repo

- These runners use your GitHub account's allotment of free minutes, and are then charged at the per minute rates. 

### Installed Softwares

[Ubuntu2204-Readme.md](https://github.com/actions/runner-images/blob/ubuntu22/20240609.1/images/ubuntu/Ubuntu2204-Readme.md)

[Customizing Github Hosted Runners](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners/customizing-github-hosted-runners)

### Large Runners Features

- More RAM, CPU, and disk space
- GPU-powered and ARM-powered runners

The following features are supported only by Windows and Linux Runners

- Static IP addresses
- Azure private networking 
- The ability to group runners
- Autoscaling to support concurrent workflows

-----
- Larger runners are not eligible for the use of included minutes on private repositories. For both private and public repositories, when larger runners are in use, they will always be billed at the per-minute rate.

### Runner Groups

Runner groups enable administrators to control access to runners at the organization and enterprise levels. With runner groups, you can collect sets of runners and create a security boundary around them. You can then decide which organizations or repositories are permitted to run jobs on those sets of machines. During the larger runner deployment process, the runner can be added to an existing group, otherwise it will join a default group.