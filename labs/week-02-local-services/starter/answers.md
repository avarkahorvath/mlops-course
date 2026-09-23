## Task 4

Postgres stores the metadata (params, metrics, tags, run lineage). MinIO stores artifacts (model files, plots, large/binary).

Saving large binary files into a relational database like Postgres would bloat the database. Postgres is meant for structured data, MinIO exists specifically to store these large files.

## Task 5

Now runs can be compared to see how the changing parameters impacted model metrics. Now, multiple members of the team can review the results from MLflow, and they do not need to rerun the code to see artifacts, results, configurations etc. It also grants reproducibility, because the parameters, metric and model files are logged (the terminal output in week1 was easily lost).