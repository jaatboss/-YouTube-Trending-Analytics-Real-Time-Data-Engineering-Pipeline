📊 YouTube Trending Analytics – Real-Time Data Engineering Pipeline

Project Overview
This project is an end-to-end real-time + batch data engineering pipeline that ingests YouTube trending video data, processes it using Kafka and Spark Structured Streaming, stores it in Delta Lake (Bronze, Silver, Gold layers), orchestrates transformations with Apache Airflow, and visualizes analytics using Streamlit.

The goal is to demonstrate modern data engineering best practices including streaming ingestion, medallion architecture, batch orchestration, and analytics-ready dashboards.

## Tech Stack

- **Language:** Python 3.10
- **Streaming:** Apache Kafka, ZooKeeper
- **Processing:** Apache Spark (Structured Streaming)
- **Storage:** Delta Lake (Bronze, Silver, Gold)
- **Orchestration:** Apache Airflow
- **Database:** PostgreSQL (Airflow metadata)
- **Visualization:** Streamlit
- **Cloud API:** YouTube Data API v3

## 🏗 Architecture Oerview

The pipeline follows a modern Medallion Architecture (Bronze → Silver → Gold):

1. **Kafka Producer** fetches trending videos from YouTube API.
2. **Kafka Topic** buffers real-time streaming data.
3. **Spark Structured Streaming** consumes Kafka data and writes raw data to Delta Bronze.
4. **Spark Batch Jobs** transform Bronze → Silver → Gold.
5. **Apache Airflow** orchestrates Spark jobs using DAG dependencies.
6. **Streamlit Dashboard** reads Gold tables for analytics visualization.

## Medallion Architecture
Bronze → Raw streaming data (append-only)
Silver → Cleaned, structured, validated data
Gold → Aggregated analytics tables for BI & dashboards

![Architecture Diagram](docs/pipeline_architecture.png)


## 🔄 Data Flow

YouTube API  
→ Kafka Producer  
→ Kafka Topic (`youtube_trending`)  
→ Spark Streaming  
→ Delta Lake (Bronze)  
→ Spark Transformations  
→ Delta Lake (Silver)  
→ Aggregations  
→ Delta Lake (Gold)  
→ Streamlit Dashboard

## 🧠 ETL Responsibilities

### Real-Time ETL
- Handled by Spark Structured Streaming
- Reads JSON messages from Kafka
- Applies schema and enrichment
- Writes append-only data to Delta Lake Bronze layer

### Batch ETL
- Orchestrated using Apache Airflow
- Bronze → Silver: data cleaning and transformation
- Silver → Gold: aggregations and analytics preparation

### Database Layer
- Delta Lake acts as the analytical data store
- Bronze: raw streaming data
- Silver: cleaned and structured data
- Gold: analytics-ready tables

## 🥇 Gold Layer Tables

- `trending_leaderboard` – Top trending videos by views and likes
- `channel_performance` – Aggregated performance metrics per channel
- `hourly_growth` – Hourly view growth analysis



## 🛫 Airflow Orchestration

The pipeline is orchestrated using Apache Airflow with three DAGs:

- `yt_bronze_to_silver` – Transforms raw Bronze data to clean Silver data
- `yt_silver_to_gold` – Builds Gold analytical tables
- `youtube_master_pipeline` – Controls execution order using TriggerDagRunOperator

The master DAG ensures Silver transformations complete before Gold processing begins.

### Airflow DAGs Overview
![Airflow DAGs](docs/airflow_dags.png)

### Master Pipeline DAG (Graph View)
![Airflow DAG Graph](docs/airflow_dag_graph.png)



## 📊 Streamlit  Dashboard

The Streamlit application visualizes Gold layer data:

- Top trending videos by views
- Channel performance metrics
- Hourly growth trends

The dashboard reads directly from Delta Lake Gold tables.

### Trending Leaderboard
![Trending Leaderboard](docs/streamlit_trending.png)

### Channel Performance & Hourly Growth
![Channel Performance](docs/streamlit_channel_performance.png)
![Hourly Growth](docs/streamlit_Hourly_Growth.png)


## How to Run the Pipeline

### 1. Start the entire pipeline
-```bash
-./start_pipeline.sh

### 2. Start Airflow
airflow webserver
airflow scheduler

### 3. Run Streamlit Dashboard
streamlit run streamlit_app/app.py

### 4. Stop the entire pipline
- ./stop_pipeline.sh



## 10. Project Structure

```md
## Project Structure

youtube-trending-analytics/
│
├── app/                    # Kafka producer
├── spark_jobs/             # Spark ETL jobs
├── delta-lake/             # Bronze / Silver / Gold tables
├── dags/                   # Airflow DAGs
├── streamlit_app/          # Dashboard
├── logs/                   # Application logs
├── start_pipeline.sh       # Pipeline startup script
└── README.md


## 👤 Author

**Suresh Kumar**  

