
# Web Bot & Attack Detection

A Big Data project focused on detecting malicious web traffic, automated bots, and suspicious activities through real-time analysis of web server access logs.

**Project ID:** IL-09  
**Domain:** Web Security / Big Data

---

## Project Overview

Web applications are constantly exposed to automated bots and malicious activities, including web scraping, brute-force attacks, and abnormal traffic patterns.

This project aims to build a Big Data platform capable of collecting, processing, and analyzing web server access logs in real time to identify suspicious behavior and potentially malicious activities.

The platform uses distributed data processing and machine learning techniques to analyze traffic patterns, detect anomalies, and provide security insights through an interactive dashboard.

## Objectives

- Collect and centralize web server access logs.
- Process incoming HTTP requests in real time.
- Identify suspicious IP addresses and abnormal traffic patterns.
- Detect potential bot activities, web scraping, and brute-force attacks.
- Analyze traffic behavior using machine learning techniques.
- Generate security reports and alerts.
- Visualize traffic statistics and suspicious activities through a dashboard.
- Identify and block malicious traffic.

## Big Data Architecture

The project follows a Big Data architecture based on the following pipeline:

**Data Sources → Apache Kafka → Data Lake (Bronze) → Apache Hudi (Silver / Gold) → Apache Spark → Dashboard**

Apache Airflow orchestrates and schedules the processing workflows.

### Main Components

| Component | Technology | Responsibility |
|---|---|---|
| Data Sources | Web Server Logs | Collect HTTP access logs |
| Data Ingestion | Apache Kafka | Stream incoming web requests |
| Data Lake | MinIO | Store raw log data |
| Lakehouse | Apache Hudi | Manage structured and historical data |
| Processing | Apache Spark | Process logs and detect anomalies |
| Machine Learning | Spark MLlib | Identify suspicious traffic patterns |
| Orchestration | Apache Airflow | Schedule and monitor workflows |
| Dashboard | Grafana | Visualize traffic and security alerts |
| Deployment | Docker Compose | Deploy the complete platform |

## Data Sources

The project will use publicly available datasets and simulated traffic:

- **Web Server Access Logs (Kaggle):** A dataset containing approximately 10 million web server log entries.
- **CSIC 2010 HTTP Dataset:** HTTP requests designed for web attack analysis and detection.

Additional simulated HTTP traffic may be generated to reproduce real-world scenarios.

## Data Processing Pipeline

The data will be organized into three layers following the Medallion Architecture:

### Bronze Layer

- Store raw web server logs.
- Preserve the original incoming data.
- Maintain the raw data for future processing and analysis.

### Silver Layer

- Parse and structure HTTP requests.
- Extract relevant information such as IP addresses, HTTP methods, URLs, response codes, and timestamps.
- Clean and transform incoming log data.
- Store processed records in Apache Hudi tables.

### Gold Layer

- Generate IP-based traffic profiles.
- Identify suspicious IP addresses.
- Produce security indicators and aggregated statistics.
- Prepare analytical datasets for visualization.

## Machine Learning & Analytics

Apache Spark will be used for distributed processing and analysis.

Planned techniques include:

- **Spark Structured Streaming:** Analyze HTTP requests using time-based windows and detect abnormal request patterns.
- **Spark MLlib – KMeans:** Identify unusual traffic behavior through clustering.
- **Classification:** Categorize traffic based on suspicious behavior.
- **Spark SQL:** Analyze targeted pages and generate security statistics.

## Dashboard

The Grafana dashboard will provide an overview of web traffic and security events, including:

- Total incoming HTTP requests.
- Traffic distribution over time.
- Most active IP addresses.
- Suspicious IP addresses.
- Potential bot activity.
- Detected anomalies and security alerts.

## Orchestration

Apache Airflow will manage the data processing workflows.

A daily DAG will be responsible for:

- Generating security reports.
- Monitoring data processing.
- Managing data maintenance and compaction.
- Preparing aggregated security indicators.

## Deployment

The complete platform will be containerized using Docker and Docker Compose.

The objective is to deploy and run all required services through a centralized `docker-compose.yml` file, simplifying environment setup and deployment.

## Team

This project is developed by:

- **Mohamed IKNIOUN**
- **Zakaria HANANI**
- **Ahmed LAARIF**
- **Abdessadek TAGARTE**

## Project Status

🚧 Currently in the initial planning and architecture phase.

---

**IL-09 — Web Bot & Attack Detection**
