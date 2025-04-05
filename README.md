# supply-chain-anomaly
Summary:

SupplySync is an intelligent, scalable platform designed to revolutionize modern supply chain operations by detecting anomalies in real-time and enabling proactive decision-making.

Built using Apache Spark Streaming (Scala), Kafka, and Hadoop, this system ingests high-velocity data streams from delivery trucks, inventory systems, and external logistics APIs to monitor and optimize last-mile delivery performance and warehouse health.

The platform leverages:

Kafka for real-time ingestion of shipment and inventory events

Spark Streaming with SparkSQL for anomaly detection based on delivery delays, inventory thresholds, and route deviations

Hadoop HDFS for integrating historical data to enrich real-time insights

Spring Boot microservices for exposing REST APIs and triggering automated resolutions

Custom dashboards and observability tools to provide clear visibility into the supply chain pulse

Innovative Edge: With event-driven architecture and scalable stream processing, SupplySync transforms passive logistics data into actionable intelligence — from detecting a truck stuck in traffic to preventing stockouts before they happen.

Impact Highlights:

Reduced anomaly detection latency from hours to seconds

Enhanced visibility across logistics and warehousing nodes

Created a reusable, microservices-based architecture for supply chain analytics

This project bridges deep technical complexity with tangible business outcomes — ideal for large-scale enterprise environments looking to modernize their supply chain with real-time intelligence.

# datasets
https://huggingface.co/datasets/infinite-dataset-hub/InventoryAnomalies?utm_source

#**Technologies used in this project**
🏗 Data Ingestion & Streaming
Apache Kafka
For real-time ingestion of shipment updates, inventory levels, and external alerts via Kafka topics.

Apache Sqoop
Used for batch ingestion from a MySQL-based warehouse inventory database into HDFS.

Kafka Producers (Scala / Python)
Custom data generators simulate IoT feeds and API inputs.

⚡ Big Data Processing
Apache Spark (Structured Streaming & Core)
Processes real-time data streams with support for batch joins and window-based aggregations.

SparkSQL
Used for querying, filtering, and detecting anomalies with expressive SQL over streaming data.

Scala
Primary language for developing Spark streaming applications.

🗄️ Storage & Data Lake
Hadoop HDFS
Stores historical warehouse and shipment data for batch processing and enrichment of live streams.

Parquet / Text Formats
Data formats used for efficient storage and retrieval from HDFS.

🧪 Anomaly Detection Logic
Custom Business Rules (Spark SQL + Scala)
Logic for detecting delays, stockouts, abnormal routes, etc.

Kafka Topic: anomaly_events
Dedicated stream for identified anomalies.

🧩 Microservices & APIs
Spring Boot (Java)
REST APIs to expose anomaly alerts, integrate with external systems, and trigger resolutions.

Apache Kafka Consumer (Java)
Reads from anomaly_events for real-time downstream processing.

📈 Visualization & Monitoring
Grafana / Kibana (optional)
Dashboards for tracking KPIs like delivery anomalies, inventory trends, and system health.

Spark UI & Kafka Monitoring Tools
For job performance and stream lag diagnostics.

🧰 DevOps & Support Tools
Docker
For local development of Kafka, Zookeeper, Spark, and Hadoop clusters.

Git / GitHub
Version control and collaboration.

Postman / Swagger
API testing and documentation.

Let me know if you’d like:

Icons and visuals for this stack (great for slides!)

A simplified version for a resume line

A breakdown of the cloud-native version of this stack (AWS/GCP/Azure)

    
