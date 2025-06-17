# ***COFFEE SALES DATA PIPELINE***
    
![Image](img/data-pipeline-v1.1.png)

## **📕 Table Of Contents**
* [⚙️ Local Setup](#local-setup)
* [⚡ Streaming Pipeline](#streaming-pipeline)
* [⏳ Batch Pipeline](#batch-pipeline)
* [📝 Documentation](index.md)
---

## **⚙️ Local Setup**

### 1. Prerequisites
- <p>Install <a href="https://www.docker.com/products/docker-desktop/" target="_blank">Docker</a>.</p>
- <p>Install <a href="https://www.python.org/" target="_blank">Python</a>.</p>
- <p>Install <a href="https://dev.mysql.com/downloads/" target="_blank">MySQL</a>.</p>

- Clone, fork, or download this GitHub repository on your local machine using the following command:
```bash
git clone https://github.com/lnynhi02/coffee-sales-data-pipeline.git
```

### 2. Project Structure
```
coffee_sales_project/
├── airflow/                              # Airflow configuration and orchestration
│   ├── dags/
│   │   └── spark_job_airflow/            # Airflow DAGs to trigger Spark jobs
│   └── Dockerfile                        # Dockerfile to build custom Airflow image
│
├── data/                                 # Static CSV files to preload into MySQL
│   ├── diamond_customers.csv
│   ├── payment_method.csv
│   ├── product_category.csv
│   ├── products.csv
│   └── stores.csv
│
├── logs/                                  # Logs for different pipeline components
│   ├── batch.log
│   ├── real-time.log
│   └── data_validation.log
│
├── scripts/                              
│   ├── database/                          # Scripts related to database setup and load
│   │   ├── create_table.py 
│   │   ├── generate_data.py
│   │   ├── load_static_file.py            # Load static CSV files into MySQL
│   │   └── lookup_data_cache.py
│   ├── update_order_status.py
│
│   ├── batch/                             # Batch processing scripts (Lakehouse architecture)
│   │   ├── bronze_dimension_fact_load.py
│   │   ├── silver_dimensions.py
│   │   ├── silver_facts.py  
│   │   ├── gold_dim_payment.py
│   │   ├── gold_dim_products.py
│   │   ├── gold_dim_stores.py 
│   │   ├── gold_fact_orders.py
│   │   ├── utils.py                       # Shared utility functions for batch jobs
│   │
│   │   └── data_quality/                  # Data validation and quality check scripts
│   │       ├── bronze_validation.py
│   │       └── silver_validation.py
│
│   └── real-time/                         # Real-time pipeline logic
│       ├── kafka_handler.py               # Kafka Handle Class
│       ├── kafka_client.py                # Handle Kafka messages for suggestion logic
│       └── mysql-src-connector.json       # Kafka Connect source connector config for MySQL
│
├── volumes/                               # Volume mount point for Docker (e.g., persistent storage)
│
├── .env                                   # Environment variables (MySQL config, SMTP, etc.)
├── docker-compose.yaml                    # Docker Compose file (real-time)
├── docker-compose-batch.yaml              # Additional Compose file (for batch processing setup)
├── mysql-connector.jar                    # MySQL JDBC connector for Spark
└── real-time.ps1                          # Windows PowerShell script to start real-time processes
```

---

1. Create a virtual environment:
   ```sh
   python -m venv venv
   ```
2. Activate the virtual environment:
      - **Windows PowerShell:**
      ```sh
      venv\Scripts\Activate
      ```
      - **Linux/macOS:**
      ```sh
      source venv/bin/activate
      ```
3. Install the necessary package:
   ```sh
   pip install -r requirement.txt
   ```
---

## **⚡ Streaming Pipeline**
Visit [this](streaming.md) for more details.

## **⏳ Batch Pipeline**
Visit [this](batch.md) for more details.

## **📝 Documentation**
Visit [this](index.md) for more details.

---
Feel free to reach out or message me if you have any questions!