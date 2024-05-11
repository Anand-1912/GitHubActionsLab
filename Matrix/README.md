## Notes

[Reference](https://docs.github.com/en/actions/using-jobs/using-a-matrix-for-your-jobs)

1. By default, matrix strategy runs the jobs in parallel.GitHub will maximize the number of jobs run in parallel depending on runner availability. To set the maximum number of jobs that can run simultaneously when using a matrix job strategy, use jobs.<job_id>.strategy.max-parallel.

[ref](https://docs.github.com/en/actions/using-jobs/using-a-matrix-for-your-jobs#defining-the-maximum-number-of-concurrent-jobs)

2. If one job fails in the matrix, the remaining un-finished jobs will be cancelled and skipped be default but this can be changed by using **continue-on-error** at the job level.

[ref](https://docs.github.com/en/actions/using-jobs/using-a-matrix-for-your-jobs#handling-failures)

3. we can use **include** (jobs.<job_id>.strategy.matrix.include) to expand existing matrix configurations or to add new configurations. The value of include is a list of objects.

[ref](https://docs.github.com/en/actions/using-jobs/using-a-matrix-for-your-jobs#expanding-or-adding-matrix-configurations)

4. To remove specific configurations defined in the matrix, use jobs.<job_id>.strategy.matrix.exclude.

[ref](https://docs.github.com/en/actions/using-jobs/using-a-matrix-for-your-jobs#excluding-matrix-configurations)
