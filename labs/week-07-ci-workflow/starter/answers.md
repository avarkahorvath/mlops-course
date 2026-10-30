
CI Url: https://github.com/avarkahorvath/mlops-course/actions/runs/41062788878
CT Url: https://github.com/avarkahorvath/mlops-course/actions/runs/41070884085

## Exercise  1

### 1.1
*What is autograde.yml for? Which commands does it run, and in which folders?*

It is the continuous integration workflow. On each PR/push/manual run, GitHub starts an ubuntu machine, and checks every lab. 
In the folders matching labs/*/starter or labs/* that have a pyproject.toml, it runs:
-  uv lock --check (checks if uv.lock is out of date with pyproject.toml)
- uv sync --locked --all-groups (installs dependencies )
- uv run --frozen pytest -q (runs the tests)


## Exercise 5


### 5.1
*What differs between ci.yml and ct.yml, and what is the same? What does the difference change: who waits for the run, is there a diff to read, and who approved the input?*

The Makefile pipeline, the gate, the summary and upload steps are the same.
The triggers differ (one is on push, other is triggered by time), plus CT does not have lint.
With CI, the dev pushing waits for the run. The scheduled CT run does not necessarily have anyone waiting on it. In CI there is a code diff reviewers can read, there is no such thing in CT. In CI code is approved by a review, in CT the new input (data) is not explicitly approved, only by the automatic data contract.

### 5.2
*ct.yml stops at the gate: it does not register, promote or deploy. Give a reason that is not about credentials. Is a GO enough to release a model?*

A GO means the model passed the rules that were written down. The gate does not check everything though, there can be edge cases/missed that can make this check uncertain.  4 slice criteria were skipped here because the groups were too small, those were not checked. Releasing should still happen by a human decision. So no, GO is not enough.

### 5.3
*Name one way a schedule: trigger can run late, or not at all, without anyone being told.*
GitHub can delay a scheduled run under high load, they can even be dropped. No one gets notified, you can only notice by checking the Actions tab.