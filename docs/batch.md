# ***⏳ Batch Pipeline***

Run the necessary containers. We add the `--build` flag because we have customized our Airflow image using a `Dockerfile` located in the `airflow/` folder.
```
docker compose -f docker-compose-batch.yaml --build -d
```

## 1. Setup Spark
You can change the `SPARK_WORKER_CORES` and `SPARK_WORKER_MEMORY` values in the `spark-worker` service depending on your available resources.
```
  x-spark-common: &spark-common
    image: bitnami/spark:3.5.1
    volumes:
      - ./scripts/batch:/opt/bitnami/spark/scripts
    networks:
      - myNetwork

  spark-master:
    <<: *spark-common
    container_name: spark-master
    command: bin/spark-class org.apache.spark.deploy.master.Master
    ports:
      - "9090:8080"
      - "7077:7077"
    networks:
      - myNetwork

  spark-worker:
    <<: *spark-common
    container_name: spark-worker
    command: bin/spark-class org.apache.spark.deploy.worker.Worker spark://spark-master:7077
    depends_on:
      - spark-master
    environment:
      SPARK_MODE: worker
      SPARK_WORKER_CORES: 4
      SPARK_WORKER_MEMORY: 4g
      SPARK_MASTER_URL: spark://spark-master:7077
    networks:
      - myNetwork
```
---

## 2. Setup Airflow
We will mount all the necessary components to run the Spark job inside the Airflow container.

```
x-airflow-common:
  &airflow-common
  build:
    context: .
    dockerfile: ./airflow/Dockerfile
  environment:
    &airflow-common-env
    TZ: Asia/Ho_Chi_Minh    # change to your local timezone
    AIRFLOW__CORE__EXECUTOR: LocalExecutor
    AIRFLOW__DATABASE__SQL_ALCHEMY_CONN: postgresql+psycopg2://airflow:airflow@postgres:5432/airflow
    AIRFLOW__CORE__LOAD_EXAMPLES: 'false'
    AIRFLOW__API__AUTH_BACKENDS: 'airflow.api.auth.backend.basic_auth,airflow.api.auth.backend.session'
    MYSQL_HOST: 'host.docker.internal'
    MYSQL_USER: ${MYSQL_USER}
    MYSQL_PASSWORD: ${MYSQL_PASSWORD}
    MYSQL_DATABASE: ${MYSQL_DATABASE}
  volumes:
    - ./airflow/dags:/opt/airflow/dags
    - ./airflow/logs:/opt/airflow/logs
    - ./scripts/batch:/opt/airflow/scripts
    - ./scripts/utils.py:/opt/airflow/scripts/utils.py
    - ./jars:/opt/airflow/jars
    - ./logs:/opt/airflow/logger
    - ./volumes/spark-cache/.ivy2:/home/airflow/.ivy2
  user: "${AIRFLOW_UID:-50000}:0"
  depends_on:
    - postgres
  networks:
    - myNetwork
```
After the Airflow webserver is up, you can access the UI by navigating to <a href="http://localhost:8080" target="_blank">localhost:8080</a>. Log in using the default credentials — both the username and password are `airflow`.<br>
![Image](img/batch/airflow-ui.png)

Once logged in, you should see the DAG named `spark_job_airflow.py` listed in the dashboard.
![Image](img/batch/airflow-dag.png)

Before running the DAG, make sure to create a Spark connection in Airflow.<br>
Set the host field to `spark://spark-master`, which corresponds to the name of the Spark master service defined in the Docker Compose setup. The port should be set to `7077`, which is the default port used by the Spark master for cluster.
![Image](img/batch/airflow-connection.png)
![Image](img/batch/spark-connection.png)

All the tasks are defined in the DAG as shown below.
![Image](img/batch/airflow-task.png)

!!! note
    Each script starts with a comment header describing its purpose.  
    Inline comments are also included to clarify key logic — feel free to explore the code directly.

!!! info
    Here are some additional points worth mentioning:

    We have implemented custom data quality checks located in the `batch/data_quality/` folder. These scripts validate the data at both the bronze and silver layers. For example:

    - **bronze_validation.py**: This script checks for schema conformity, non-null constraints, and basic data integrity.
    The reason for performing schema validation at this stage is to catch issues early—such as unexpected schema changes—before the data flows into downstream layers. This early detection saves time and effort required for backfilling in case of errors.

    - **silver_validation.py**: This script uses Deequ to enforce additional data quality constraints, such as non-null checks and domain-specific validations on certain columns.

## 3. Setup Minio
Go to <a href="http://localhost:9001" target="_blank">localhost:9001</a> and log in using the default credentials — `minioadmin` for both the username and password.
![Image](img/batch/minio-login.png)

Create the following buckets: `bronze-layer`, `silver-layer`, and `gold-layer`.
![Image](img/batch/minio-bucket.png)

## 4. Run the Batch Pipeline
Now we can execute the DAG
![Image](img/batch/run-dag.png)

You can check the execution logs in the `logs/` folder.
![Image](img/batch/log-1.png)
![Image](img/batch/log-2.png)

Go to <a href="http://localhost:9001" target="_blank">Minio UI</a> to view the data.
![Image](img/batch/bronze.png)
![Image](img/batch/silver.png)
![Image](img/batch/gold.png)

Once the DAG finishes successfully, click the `show_gold_layer_data` task, then open the `logs` tab to explore the schemas and sample output of each table.
![Image](img/batch/task_success.png)
![Image](img/batch/dim_products_schema.png)
![Image](img/batch/dim_products_table.png)
![Image](img/batch/fact_orders_schema.png)
![Image](img/batch/fact_orders_table.png)