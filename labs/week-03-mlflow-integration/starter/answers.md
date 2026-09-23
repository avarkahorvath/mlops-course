## Task4

### 1.
They disagree. `rf-n_estimators=300` has the best f1 score, and `logreg-C=1.0` as the best roc-auc.
ROC AUC  evaluates how well positive and negative classes are separated overall. f1 requires a fixed threshold to turn probabilities into hard class labels before calculating the harmonic mean of precision and recall. 

### 2

I pick 810657afab904f128bf25781f690d5e6 (`logreg-C=1.0`). It was the winner of roc-auc, so it is better at seperating between classes accross all tresholds. A higher f1 score can be achieved later by adjusting the decision treshold. Logistic regession is more interpretable and outputs clear coefficients, which is a pro for the dataset used.

### 3

MLflow  recorded the `git_commit` tag, the  duration of runs, and saved the confusion matrices aswell.

## Task6

### 1

1. alias   -> version   
`client.get_model_version_by_alias(name="diabetes-classifier", alias="staging")`

2. version -> run      
`client.get_run(run_id=model_version.run_id)`

3. run     -> evidence   
Reads `run.data.tags["git_commit"], run.data.params, run.data.metrics` from the returned run


4. commit  -> code      
git call, `git diff --stat <git_commit>`
      


### 2
Part 4 showed a non-empty diff, because there were uncommitted changes in the code.  The commit hash was unreliable. 
git_dirty records whether or not the working tree is dirty. The commit hash becomes reliable. 
If it is dirty, then the run still cannot be reproduced, it does not fix this problem.

### 3
It should refuse. If it would not refuse, then the trail points to a commit which is not reliable. It hurts reproducibility.
Enforcing it damages efficiency, and developer speed. Committing before each means comitting half-winished work.

### 4
You can use multiple aliases at once. You can use your own names. You can reassign without touching versions. Moving an alias does not touch the immutable version  record.

### 5
Git is not meant to track data, so in real situations, it would not even be caught by git_dirty. git_dirty could be false, while the data changed.
That means that the metrics have a chance of not being reproducible,and the current code would not even catch it. The chain would still look clean.
Logging a dataset hash or tracking data/seperate data versioning  could fix this.

## Task7

### 1
The @staging an and @champion aliases moved from v3 to v2. The versions stayed the same. The server loading models:/diabetes-classifier@champion now shows v2.

### 2
The roll_back functions is writing rolled_back_at to the record, an auditor could learn it from tags.
Without tags, they would not have evidence that v3 was ever champion.

### 3

Yes,  roll_back checks for promoted_at. v2 was promoted previously, so it has proof that once, it passed the validation gate already, so it is a good target.