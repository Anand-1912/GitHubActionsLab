## Notes

1. Custom Actions can simplify our workflows and they are reusable in multiple workflows.

 - [ref](https://docs.github.com/en/actions/creating-actions/about-custom-actions)

 - [metadata syntax](https://docs.github.com/en/actions/creating-actions/metadata-syntax-for-github-actions#runs-for-javascript-actions)

2. Types of Custom Actions

   - Javascript Actions
   - Docker Actions
   - Composite Actions

**NOTE**

How containers can be used in GitHub Actions?

 - Container Actions

 - We can run an entire in the container. (Job Containers)
   - [ref](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idcontainer)

 - we can define *Service containers* for a job.
   - [ref](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idservices)

When we use containers to run a Job or an Action, they have to be executed inside a Linux Machine irrespective of whether we are using Github Hosted runners or Self hosted runners for the runner host.