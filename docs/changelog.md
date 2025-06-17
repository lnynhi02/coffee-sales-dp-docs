# ***📌 Changelog***

### Version 1.0 - June 4th, 2025
- Kafka Connect for CDC (MongoDB → ElasticSearch).
- Real-time dashboard built with Kibana.
- Batch loading via Airbyte (full load).
- Data warehouse: PostgreSQL.
- Transformation & testing via DBT.

### Version 1.1 - June 16th, 2025

#### Added
- Kafka Consumers for real-time business logic.
- Redis caching layer.
- Airflow orchestration.
- SCD Type 2 logic.
- Monitoring & alerting via Prometheus + Grafana.

#### Changed
- Source DB: MongoDB → MySQL.
- Batch: Airbyte → Spark (incremental).
- Data validation: DBT → Deequ + PySpark.
- Storage: PostgreSQL → MinIO (Lakehouse).

#### Removed
- ElasticSearch + Kibana stack.
- DBT transformation and testing.