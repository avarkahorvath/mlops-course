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

### 5.2
*ct.yml stops at the gate: it does not register, promote or deploy. Give a reason that is not about credentials. Is a GO enough to release a model?*

### 5.3
*Name one way a schedule: trigger can run late, or not at all, without anyone being told.*
