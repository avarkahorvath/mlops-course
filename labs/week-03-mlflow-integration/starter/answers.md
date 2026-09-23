## Task4

### 1.
They disagree. `rf-n_estimators=300` has the best f1 score, and `logreg-C=1.0` as the best roc-auc.
ROC AUC  evaluates how well positive and negative classes are separated overall. f1 requires a fixed threshold to turn probabilities into hard class labels before calculating the harmonic mean of precision and recall. 

### 2

I pick 810657afab904f128bf25781f690d5e6 (`logreg-C=1.0`). It was the winner of roc-auc, so it is better at seperating between classes accross all tresholds. A higher f1 score can be achieved later by adjusting the decision treshold. Logistic regession is more interpretable and outputs clear coefficients, which is a pro for the dataset used.

### 3

MLflow  recorded the `git_commit` tag, the  duration of runs, and saved the confusion matrices aswell.

## Task6