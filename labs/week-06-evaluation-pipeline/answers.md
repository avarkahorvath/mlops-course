## Exercise  2

### 2.1
*Which group is the model worst on, and by how much against the aggregate? How far can you trust that gap, given the group's n?*

For age_band, the worst group is 50+ on almost every metric, it underperforms when compared to the other groups in age_band. accuracy is 0.1867 worse, f1 is 0.1341 worse, recall is 0.0107 worse, roc_auc is 0.2274 worse. Roc auc is the most affected metric here, because it is treshold independent, and this drop compared to the aggregate is substancial. 19 data rows are enough to take seriously, but it is not enough to trust that gap yet, a few new patients could influence the scores substancially.

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

## Exercise 4

### 4.1
*Week 5's gate worked because prepare depended on the verdict file. Why can that not work for this gate, and what replaces it?*

Previously, the gate checked the data, the order was validate->prepare->train. If the data was bad, then prepare did not even start, the gate stopped the pipeline before train.
Now, the gate checks the model, the order is train->eval->gate, when the gate is checked, the model already exists. The gate is the last step, no stage can depend on this.
This protects the model going into the registry still, so it cannot be released. register and promote call require_go, so these do not release the model unless it's 'go'.

### 4.2
*What happens if register does not call require_go? Who would notice, and when?*
The no-go model would be registered and promoted, because nothing checks the verdict. No error appears, no one would notice right away, only later, when the model starts performing worse in real life. 

### 4.3
*With C: 0.005, F1 fell by 0.1098 and ROC AUC rose. Which one is the right answer? What do you need to know about how the model is used, and what does acceptance.regression.primary_metric commit you to?*

Both are correct, depending on the use of the model. ROC AUC rising means that the new model ranks sick patients above healthy ones a bit better. F1 falling means that at the 0.5 threshold, it catches fewer diabetics.

If it is possible to change the threshold, the better ROC AUC matters more and the new model could be OK. But if the model has a fixed treshold at at 0.5, F1 matters more and the new model is worse. 
To get to a conclusion, we need to know if the threshold is fixed, and whether missing a diabetic is worse than a false alarm.

I set the primary metric to f1, which commits me to judge the model by its yes/no decisions at 0.5. So the gate blocks this model, even though it ranks better.


## Exercise 5

### 5.1
*age_band has a selection-rate gap over 0.30, yet you gate only its TPR gap. Why is the large selection-rate gap defensible?*
 Diabetes is more common in older patients, so it is normal that the model flags them more often, this reflects real life. The thing that matters here is that the truly sick people are caught equally, and that is the TPR gap.

### 5.2
*The TPR limit on age_band is looser than on insulin_missing. Why, and what would make you tighten it?*

It is because the groups are not equal, insulin_missing has 2 big groups while age_band has 4 groups. When there are only a few positives, even just one patient moves TPR a lot, the age gap is noiser for this reason. I would tighten it if I had more data, especially for these subgroups (so more 50+ diabetes patients).

### 5.3
*This dataset has no sex, ethnicity, income or clinic column. Name one group you would want to audit this model for and cannot. What would have had to happen, and when?*
First, we have to look at the origin of the dataset. The Pima dataset has only female and Indian women, so I can rule out sex and ethnicity. 

I would pick income, a poorer patient might be unfairly misdiagnosed with the current model and I would not even realize it. They might get fewer tests or get sick easier because of the lack of healthy food/a worse lifestyle.
 When the data was recorded, income could have been collected aswell. It is almost impossible to collect it later, because of the strict health organization guidelines, data is (hopefully) anonymized, so we cannot get the data later by asking the patients. 