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