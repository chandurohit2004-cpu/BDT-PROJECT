# Student Education Performance Analytics
An advanced analytics dashboard and ETL pipeline to predict, monitor, and report on student performance and burnout risk. This solution leverages modern data engineering (Spark), machine learning (Decision Trees), and time-series monitoring (InfluxDB/Grafana) to provide educators with actionable real-time insights.
---
## 🏗️ Architecture & Tech Stack
- **Frontend/Backend:** Flask (`server.py`), HTML/Vanilla CSS/JS (Dashboard UI)
- **Data Engineering (ETL):** Apache Spark / PySpark
- **Database (Relational):** SQLite 3 (`students.db`)
- **Time-Series Monitoring:** InfluxDB & Grafana (via Docker)
- **Machine Learning:** Scikit-Learn (Decision Tree Classifier)
- **Data Formats:** Parquet (Gold Analytical Layer), CSV
---
## ⚙️ Prerequisites
Before you begin, ensure you have the following installed on your machine:
1. **Python 3.8+**
2. **Docker & Docker Compose** (required for InfluxDB and Grafana)
3. **Apache Spark** (and PySpark)
4. **Python Dependencies:** (Install via `pip install -r requirements.txt` if available, or manually install `flask`, `pandas`, `pyspark`, `scikit-learn`, `influxdb-client`, `requests`, `matplotlib`, `seaborn`)
---
## 🚀 Setup & Installation (How to Run)
Follow these steps strictly in order to initialize the database, generate data, train the model, and spin up the dashboard.
### Step 1: Start Infrastructure (InfluxDB & Grafana)
Open a terminal in the project root and spin up the Docker containers in the background:
```bash
docker-compose up -d
```
*(Wait a few seconds for the databases to initialize).*
### Step 2: Generate & Process Data (The ETL Pipeline)
Generate the base 1,000 record dataset, migrate it to the SQLite database, and run the Spark ETL job to create the analytical "Gold Layer" Parquet files:
```bash
python generate_1000_students.py
python migrate_to_sql.py
python spark_etl_process.py
```
### Step 3: Train the Machine Learning Model
Transform the data for machine learning and train the Decision Tree model to predict student risk/grades:
```bash
python data_transformation.py
python model_training.py
```
### Step 4: Sync Analytics to InfluxDB
Push the processed analytical data to InfluxDB so Grafana can visualize it:
```bash
python spark_to_influx_grafana.py --once
```
### Step 5: Start the Web Application
Launch the Flask backend server:
```bash
python server.py
```
> **Access the Dashboard:** Open your web browser and go to `http://localhost:5001`
> **Default Login:** Username: `admin` | Password: `password123`
---
## 📂 Project Structure Overview
```text
Student education performance/
├── server.py                   # Main Flask backend application providing REST APIs
├── generate_1000_students.py   # Script to generate synthetic student data (CSV)
├── migrate_to_sql.py           # Migrates data from CSV to the SQLite database
├── spark_etl_process.py        # Spark job to process data into a Gold Parquet layer
├── data_transformation.py      # Prepares features for the ML model
├── model_training.py           # Trains the Decision Tree model (outputs .pkl & .png)
├── spark_to_influx_grafana.py  # Syncs Parquet analytics to InfluxDB
├── docker-compose.yml          # Infrastructure for InfluxDB & Grafana
├── static/ & templates/        # Frontend UI files (HTML, CSS, JS, images)
├── students.db                 # Main SQLite Database
├── student_analytics_gold.parquet # Output of the Spark ETL process
└── student_performance_model.pkl  # Trained ML model weights
```
---
## ✨ Key Features
- **Real-time Processing:** Seamlessly handles and processes 1,000+ student records.
- **Predictive Analytics:** Automated Machine Learning pipeline predicts at-risk students and estimates grades based on attendance, internal marks, and external marks.
- **Data Engineering:** Uses Apache Spark to create a highly optimized "Gold Layer" Parquet generation for fast analytics querying.
- **Live Monitoring:** Live Grafana dashboard synchronization for system and structural health metrics.
- **Dynamic Dashboard:** A rich, responsive UI providing insights on department averages, individual grade distributions, and automated burnout detection.
