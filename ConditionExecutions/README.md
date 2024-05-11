## Controlling Execution Flows

1. By default, if a step fails in a job, the entire job fails.

2. If a Job fails, then it's dependent jobs will also fail.

3. But this behaviour can be changed in github actions workflows.

4. for example, we can ensure that a particular step is excecuted only when an another step before this step is failed.

## Conditional Executions

1. using **if** field

   can be used with Jobs and Steps

2. using **continue-on-error**

   can be used with Steps

   ```
      eg: - name: Print Hello
            continue-on-error: true
            id: hello-step
            run : |
             echo "Hello"
   ```

---

#### Status Check Functions

---

[reference](https://docs.github.com/en/actions/learn-github-actions/expressions#status-check-functions)

**success**

Returns true when all previous steps have succeeded.

**always**

Causes the step to always execute, and returns true, even when canceled

> [!WARNING]  
> Avoid using always for any task that could suffer from a critical failure, for example: getting sources, otherwise the workflow may hang until it times out. If you want to run a job or step regardless of its success or failure, use the recommended alternative: if: ${{ !cancelled() }}

**cancelled**

Returns true if the workflow was canceled.

**failure**

Returns true when any previous step of a job fails. If you have a chain of dependent jobs, failure() returns true if any ancestor job fails.
