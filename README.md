# Restaurant Analytics DWH

### About the Project
This project is a solution for building a centralized Data Warehouse (DWH) tailored for the restaurant business. The primary objective is to automate data ingestion, transformation, and visualization to enable real-time monitoring of Key Performance Indicators (KPIs).

### Roadmap
* [x] Infrastructure setup (Docker Compose).
* [x] PostgreSQL database deployment.
* [x] ClickHouse database deployment.
* [x] Pipeline orchestration setup using Apache Airflow.
* [ ] In progress: Migration of SQL transformations to dbt (for data quality control and data lineage tracking).
* [ ] Planned: Expanding the DWH with an inventory management domain: integrating stock balances, standard recipe cards, and Food Cost calculations.
* [ ] Planned: Architecture scaling to integrate external data sources (delivery aggregators) and support a multi-restaurant network.

### Technology Stack
* **Orchestration:** Apache Airflow
* **Data Warehouse (DWH):** ClickHouse
* **Data Source:** PostgreSQL
* **BI & Visualization:** Preset (Apache Superset)
* **Infrastructure:** Docker, Docker Compose

### Solution Architecture
1. **Data Generation:** Automated generation of transactional data (receipts) in PostgreSQL.
2. **ETL Processes:** Apache Airflow orchestrates data generation scripts and data processing pipelines.
3. **Data Layers:** 
    * **Bronze Layer:** Raw data extracted from PostgreSQL via the ClickHouse Engine.
    * **Silver Layer:** Data cleaning, normalization, and standardization.
    * **Gold Layer:** Data marts ready for business intelligence and analytics.
4. **BI Layer:** Connecting data marts to Preset for interactive visualization.

### Business Value
The implemented DWH solution automates data collection and the calculation of key restaurant metrics, eliminating manual effort and minimizing human error. The finalized data marts (Gold Layer) consolidate the monitoring of financial metrics and staff productivity into a single interactive dashboard. This provides the business with a reliable Single Source of Truth (SSOT) for daily data-driven decision-making.

### BI Layer and Reporting
The dashboard is built using Preset (Apache Superset). Data is queried directly from the Gold Layer data marts in ClickHouse.
The data in the screenshot displays daily revenue, receipt counts, and revenue per employee for June 2026.
![Dashboard](images/preset_dashboard.png)

### Getting Started (Local Deployment)
**Prerequisites:**
To run this project locally, ensure you have the following installed:
* **Docker** and **Docker Compose**
* **Git**

**Step-by-Step Guide:**
1. **Clone the repository:**
    ```bash
    git clone https://github.com/AnttiPakkanen/restaurant_project.git
    cd restaurant_project
    ```
2. **Run the initialization script:**
    ```bash
    ./init.sh
    ```
3. **Open the .env file in any text editor and set your passwords for PostgreSQL and ClickHouse.**
4. **Launch the infrastructure:**
    Spin up all necessary services (databases and Airflow) with a single command:
    ```bash
    docker compose up -d
    ```
5. **Initialize the ETL process:**
    * Access the Airflow web interface at http://localhost:8080 (default credentials: airflow / airflow).
    * Locate the receipts_daily_load DAG in the DAGs list.
    * Unpause it (toggle to the 'On' position) and run it manually (Trigger DAG) to generate data in PostgreSQL and load it into ClickHouse.
6. **Stop the services:**
    To stop the project and free up system resources, run:
    ```bash
    docker compose down
    ```