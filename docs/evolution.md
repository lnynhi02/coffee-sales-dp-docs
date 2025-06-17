# ***PROJECT EVOLUTION***

This document outlines the evolution of the Coffee Sales Data Pipeline project. It highlights key architectural, technological, and business logic changes across major versions. This helps track improvements over time and maintain clear context for future development.

---

#### v1.0 - 2025.04 (Initial Release)

<span style="font-weight:620; font-size: 17px;">🎯 Overview</span><br>
A basic data pipeline to simulate and analyze coffee shop sales.

![Pipeline Diagram](img/data-pipeline-v1.0.png)

<span style="font-weight:620; font-size: 17px;">📦 Architecture</span> 

<span style="font-weight:620; font-size: 15px;">Data Source</span>

- `MongoDB` stores transactional data (coffee orders).

<span style="font-weight:620; font-size: 15px;">Real-time Processing</span>

- `Kafka Connect` captures changes from MongoDB.
- `ElasticSearch` stores real-time events.
- `Kibana` visualizes operational metrics and trends.

<span style="font-weight:620; font-size: 15px;">Batch Processing</span>

- `Airbyte` extracts data from MongoDB into PostgreSQL (raw layer).
- `PostgreSQL` acts as the data warehouse for storing structured data.
- `DBT` transforms raw data into analytics-ready models and ensures data quality through testing.

<span style="font-weight:620; font-size: 15px;">Load Strategy</span>

- Full load only; no incremental load implemented yet.

<span style="font-weight:620; font-size: 17px;">✅ Benefits</span>

- Easy setup.
- Low code.
- Schema flexibility, easy to scale with MongoDB.
- Airbyte supports schema changes and multi-source ingestion.

<span style="font-weight:620; font-size: 17px;">⚠️ Limitations</span>

- Only supports full-load batch processing; no incremental logic.
- Does not implement Slowly Changing Dimension (SCD) handling.
- Lacks real-time business logic (e.g., alerting, rule-based processing).
- Orchestration tools are not integrated.
- Logging and monitoring are not yet implemented.
---

#### v1.1 - 2025.06

<span style="font-weight:620; font-size: 17px;">🎯 Overview</span><br>
Upgraded to support real-time business rules, historical data tracking, and scalable lakehouse design.

![Image](img/data-pipeline-v1.1.png)

<span style="font-weight:620; font-size: 17px;">📦 Architecture</span> 

<span style="font-weight:620; font-size: 15px;">Data Source</span>

- `MySQL` stores both transactional and attribute data.

<span style="font-weight:620; font-size: 15px;">Real-time Processing</span>

- Continues to use `Kafka Connect` for Change Data Capture (CDC).  
- Adds `Kafka Consumers` to handle real-time business logic.  
- Uses `Redis` for low-latency lookups and caching.
- Uses `Prometheus` and `Grafana` to observe Kafka health and trigger alerts.

<span style="font-weight:620; font-size: 15px;">Batch Processing</span>

- Adopts a Lakehouse and Medallion Architecture.  
- `Spark` handles data ingestion, transformation, and data quality checks.  
- `Airflow` is used for orchestration and job scheduling.

<span style="font-weight:620; font-size: 15px;">Load Strategy</span>

- Supports incremental load.

<span style="font-weight:620; font-size: 17px;">✅ Benefits</span>

- Incremental load implemented.  
- Slowly Changing Dimension (Type 2) supported.  
- Real-time business rules applied during stream processing.  
- Monitoring, alerting and logging integrated.  
- Full orchestration with Airflow.

<span style="font-weight:620; font-size: 17px;">⚠️ Limitations</span>

- Not yet serving layer (e.g., SQL engine or BI tool) for DA/BA usage.

---

Last updated: 16, June 2025
