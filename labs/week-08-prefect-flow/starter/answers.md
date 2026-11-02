## Exercise 2

### 2.1
*Name three capabilities Prefect provides for this lab that DVC does not, and one capability DVC provides that Prefect does not. Explain which problem each capability solves.*

Prefect provides, DVC does not:
- Network tasks are retried with a growing delay. This solves temporary failures, because now, they do not fail the whole run.
- The flows and runs are stored with states, durations and logs. This makes looking for the error retrospectively got easier.
- Scheduling is available, so cron starts the training on its own. This is a better alternative to manual training if we do it on the regular anyway.

DVC provides, Prefect does not:
- DVC hashes the inputs of stages, and that is how it can re-run only what has changed. This means if a stage has not changed, there is no need for a re-run - this saves time, and resources. 

### 2.2
*run_pipeline calls dvc repro as a single task. What would you lose by replacing it with one Prefect task per DVC stage? When might splitting the stages into separate tasks be useful?*


Prefect would run  every stage, DVC could potentially skip stages if they do not need to be re-ran. This could also cause drift, because the dependency graph is already defined in dvc.yaml. Recreating these as Prefect tasks would mean we need to write the same order in code, if we do not want drift.


Splitting could be useful for different environments. If train is resource consuming (needs GPU/TPU, which is expensive), but prepare is not, then seperating them and running them in different environments makes sense.

