## Controlling Execution Flows

1. By default, if a step fails in a job, the entire job fails.

2. If a Job fails, then it's dependent jobs will also fail.

3. But this behaviour can be changed in github actions workflows.

4. for example, we can ensure that a particular step is excecuted only when an another step before this step is failed.

## Conditional Executions

1. using **if** field

   can be used with Jobs and Steps

2. using **continue-on-error**

   can be used with Jobs (in matrix strategy) and Steps

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

>[!IMPORTANT]
>In GitHub Actions, if a workflow is queued but then cancelled before it actually starts executing, none of the steps within that workflow will run, even if they are configured to always execute using status check functions.

However, if the workflow has started and is subsequently cancelled, the **if: always()** condition can ensure that certain steps are executed during the cancellation process. 

Here’s how it works:

**Queued but Not Started**: If a workflow is still in the queue and hasn't begun executing any steps, and it gets cancelled, none of the steps in that workflow will run.

**Started and Then Cancelled**: If a workflow starts running and then gets cancelled partway through, steps configured with if: always() will run. This is useful for performing cleanup tasks or ensuring certain final steps always execute regardless of the outcome of previous steps or cancellation.