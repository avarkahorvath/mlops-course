## Exercise  2

### 2.1
*Which group is the model worst on, and by how much against the aggregate? How far can you trust that gap, given the group's n?*

For age_band, the worst group is 50+ on almost every metric, it underperforms when compared to the other groups in age_band. accuracy is 0.1867 worse, f1 is 0.1341 worse, recall is 0.0107 worse, roc_auc is 0.2274 worse. Roc auc is the most meaningful metric here, because it is treshold independent, and this drop compared to the aggregate is substancial. 19 data rows are enough to take seriously, but it is not enough to trust that gap yet, a few new patients could influence the scores substancially.

bmi band has even worse regarding scores, but it has only 2 instances, so the scores do not imply any reliable finding here.


## 2.2
*What can you do with this finding now, and what would you need before you act on it?*

If we do not have more data, we can flag this and document the gap. We can decide if this is severe enough to block the release (check if this fails the acceptance gate). We can ask a domain expert if there is a reason why these patients score worse.

So before acting on it, more data is needed on 50+ patients specifically, 19 patients is too small to be sure that the gap is really our fault. We need an answer if this performance is acceptable for this subgroup or not.

## 2.3
*One group has an F1 of 0.0000 and a ROC AUC of 1.0000 on the same patients. How can both be correct, and what do they tell you about that group?*

There is only 1 positive in that subset.
The 0 f1 means that for that 1 case, the model predicted negative, it did not catch it. 
Roc auc is treshold independent, it only checks if the model ranked the positive patient above all the other patients. A 1 score means it did, which shows the model has some sort of signal for the patient.  30 patients and 1 positive case is still not enough to call it completely reliable thoigh.
