**Scheduled run:** literate-baboon - Completed

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


Splitting could be useful for different environments. If train is resource consuming (needs GPU/TPU, which is expensive), but prepare is not, then separating them and running them in different environments makes sense.

## Exercise 3

### 3.1
*Why might retries=3 on run_pipeline be unsuitable?*
Pipeline failures are deterministic, so retries are a waste, most likely they will fail identically.

### 3.2
*Retrying is safe only if running the same operation twice does no harm. Which of the tasks that retry is not safe to repeat? What would a retry do there, and how much does the existing check in that task protect you*
register_model is not safe to repeat. If mlflow commits the model version but then the response is lost, the retry registers an identical version.
The check looks for an existing version first, narrowing the possibility of this happening, but does not fully protect.

## Exercise 5

### 5.1
Is passing the quality gate enough to release a model trained by a scheduled job? What other checks or approvals would you require before it reaches users?
No. The gate is checking metrics against thresholds. I would require a human review and a comparison with the current model.

### 5.2
*What should be notified, and to whom?*
Every run's result, data version, params. If its a NO-GO/FAILED, it should go to the model owners, GO should go to the release approver.

### 5.3
*What should happen on the second failure in a row that should not happen on the first?*
The second fail confirms it is probably a persistent problem, so the schedule should be paused, otherwise it will keep failing.

### 5.4
*Which scheduled-job failure would these checks and notifications still miss?*
If the run never starts at all, then nothing notifies, because nothing fails/gets a GO/NOGO status. 
