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
