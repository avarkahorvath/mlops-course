# Task 2

## 1.
In the last commit, git gained data/.gitignore, data/raw/batch_01.csv and the pointer file. The of the pointer file alone is 97 bytes, 
SIlo gained the measurements.csv, size is 19034 bytes.
Git is designed for code or text, not large data files. This way, there is a pointer for the data in git, and the actual data is in Silo. This ensures data versioning without bloating the repo. 

## 2.
 -1. It calls bath_paths(), which ensures the batches are read in the same filename order, every time, on every machine.
 -2. build_measurements() ensures row numbers are not written (with index=false in 'to_csv'), so output is always only the data. 
 - right now, .gitattributes forces LF, but a different e nd of line character would break the md5.

## 3.
dvc pull data/measurements.csv
 - 1. have creds for silo
 - 2. they have the correct silo endpoint, make dvc-init was run and committed

 # Task 4

 ## 1.
 git checkout - It changed the pointer file on disk, swapped the md5 to v1 (from v3). The actual data file was unchanged.
 dvc checkout - It read the new pointer, and based on that, replaced the actual data file with v1's content.

 measurements.csv is gitignored, git cannot track it. DVC has no concept of git history. Git has to pick the pointer, and DVC has to get the data based on that.

 ## 2.
 git checkout would be successful, the pointer would change. At that point, v1 would not be in cache, it would fail with "ERROR: Checkout failed for following targets: [..] Is your cache up to date?".

 # Task 5

 ## 1.
      - data/processed/test.csv
      - models/model.pkl
      - models/mlflow_run_id.json
      - src/week_04_dvc_introduction/pipeline.py
      - src/week_04_dvc_introduction/model.py

If data/processed/test.csv was missing, the pipeline would still run, but could produce the wrong result. Evaluate scores the data on test.csv, and if only the test data changes (and nothing apart from that), DVC would not notice. So this step would not be run again and the scores would be kept from the old test set.


 ## 2.
My results:
Path                  accuracy    f1      precision    recall    roc_auc
metrics/metrics.json  0.7656      0.6154  0.72         0.5373    0.8183

Here, the train-test split is different because of the order of rows. v3 is built by merging the 3 batches together in arrival order.  The overall content is the same, but the order differs, so the train/test differs too, which influences the results.

# Task 6

## 1.
MLflow's 8 characters is a short fingerprint of the dataset. DVC's 32 characters hash the exact bytes, and also serves as an address to that file. I would give the auditor the md5, because with that, the object is able to be fetched from the remote, and the hash can be reproduced. Mlflow is good for a quick check, to see if two runs used the same logged dataset.

## 2.

tags.dvc_md5 = 'a8fd7b4f0d6d1bc4e378a8f76c5fff0c'
It finds runs that trained on this data version. Week3 only logged surface level info about the data that could stay the same even if the content changed.  It could not tell 2 versions of the data apart.


## 3.
The new line is :
5. Data version : a8fd7b4f0d6d1bc4e378a8f76c5fff0c
   Data location

The chain is incomplete. The run does not record whether the file on disk really matches the pointer. If the data was edited, but not dvc added, it would carry the old and incorrect md5, and no one would be able to tell from the trace.