## Task2

### 1.
*The batch has a notes column that the contract does not name. The clinic says it is free text and will come in every batch from now on. Would you add it to the contract, drop it before validation, or keep rejecting the batch? Decide, and say who must agree.*

I would probably add it to the contract as an optional column (since it is free text, and adding a note to a column is not necessary). Dropping it before validation would mean that we could lose important context to interpret unusual rows. Rejecting a batch because of an extra column is not a good idea here. I would not train the model on this data.

The company of the data source must confirm that this data does not contain any sensitive information about patients/workers/etc.  They should also clarify the meaning of this column, including what kinds of notes these are (for exmample, past history of the patient, an explanation for a missing value, etc.)
The ML engineer should also accept it, because this change affects the state of the data, they should know if they want to use this for training or take it out beforehand.

### 2.
*List the four faults in the batch: for each, the row, the column and the bad value. The report has 7 lines for 4 faults. Which fault caused more than one line, and why?*

 - `all rows - notes - column_in_schema` means that an unexpected column, notes was present in the new data.

 - `3 - glucose - unknown` means that the value in the column did not match the pre established type. The value was "unknown", which is not a float.

 - `23 - bmi - 280` The value of the column was greater than expected (100).


 - `7 - age - 250` The value of the column was greater than expected (120).
 

 ## Task4

### 1.
*Compare the two runs of make gate-fail. Without the gate, which files did the row with age = 250 reach, and who or what would use them next?*

In the older version, it ran the prepare, train and evaluate. The md5 changed, because the age=250 row ended up in train.csv or test.csv. The former means a model would train on this row, the latter would mean that it will be used to evaluate the model. 

In the newer version, validate fails, so the train.csv md5 stays the same. 

Without the gate, they 
### 2. 
*validate has failed. A teammate in a hurry runs uv run python src/main.py prepare by hand. What happens, and which line of your code makes it happen? Why is the edge in dvc.yaml not enough on its own?*

Even by hand, it will not work. _require_validated(settings) in prepare, pipeline.py reads validation.json and raises if passed is false.

### 3.
*You raise MAX_AGE from 120 to 125. Predict which stages make repro runs and which it skips. Try it, then set the value back and run make repro again. Explain why each stage ran or skipped, and say whether the model needed to be trained again.*

validate reruns, one of its deps is schemas.py. prepare skips, because the deps and inputs are unchanged. train and evaluate would rerun, because both have schemas.py as a dep. The model would have to be trained again.
