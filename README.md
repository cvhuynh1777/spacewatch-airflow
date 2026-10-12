# spacewatch-airflow

## Orchestration

The pipeline runs as a Databricks Job (`spacewatch_daily`) with four chained tasks:
bronze → silver → gold → embeddings.

**Airflow integration:** I built an Apache Airflow DAG ([spacewatch-airflow](https://github.com/cvhuynh1777/spacewatch-airflow))
that triggers this job daily using the Databricks provider's `DatabricksRunNowOperator`.
It runs locally via the Astro CLI and Docker, authenticates through an Airflow connection
(no secrets in code), retries failed runs, and waits for the Databricks run to finish
so Airflow reflects the job's true status.

**Production scheduling:** Because Airflow runs locally and the project is on Databricks
Free Edition, daily refreshes use the Databricks Job's built-in scheduler (6:00 AM Pacific).
The Airflow DAG is kept for development and demos, and can take over scheduling
if Airflow is deployed (e.g. Astronomer, MWAA, or self-hosted).
