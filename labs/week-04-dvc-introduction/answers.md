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

 