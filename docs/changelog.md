# ***📌 Changelog***

| Date       | Version | Changes                                                                                                                                     | Notes                                                                                     |
|------------|---------|---------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|
| 2025-04-04    | v1.0    | | - This marks the first version of the pipeline, in which the core logic has been successfully implemented. |


##### Version 1.0
<span style="font-weight:620; font-size: 14px;">✨ Features</span> 

- Successfully implemented an end-to-end data pipeline with two branches: batch and real-time processing.
- Developed a real-time dashboard to analyze coffee sales trends and customer behavior.
- Implemented batch processing to store historical data and generate weekly reports.
- Built a data warehouse to support analytics and reporting.
- Applied data quality checks to ensure consistency and reliability across the pipeline.

<span style="font-weight:620; font-size: 14px;">✅ Benefits</span>

- Combines real-time processing for trend analysis with batch processing for historical reporting and business insights, providing flexibility and speed for decision-making.
- CDC-based architecture ensures timely and accurate data sync.
- Low-code pipeline using Kafka Connect for real-time data transfer between sources and destinations, and Airbyte for scheduled batch ingestion, simplifying data integration and reducing development time.
- MongoDB supports schema flexibility, fast write performance, and easy scalability for handling large datasets.


<span style="font-weight:620; font-size: 14px;">⚠️ Limitations</span> 

- Airbyte Cloud currently operates in full refresh mode instead of incremental loading, as the incremental setup is still under exploration.
- The Python script for generating synthetic coffee sales data requires manual execution.
- Performance optimization has not been applied, and system-level logging/monitoring is not yet implemented.
- Encountered difficulties connecting cloud-based services (e.g., Airbyte Cloud) with local tools like PostgreSQL due to network and authentication constraints.
- Security configurations are minimal; more attention is needed to access control, data encryption, and secure communication when working with cloud services.