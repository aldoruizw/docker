# 🛫 Apache Airflow Docker Setup

Easily orchestrate workflows using [Apache Airflow](https://airflow.apache.org/) with Docker and PostgreSQL on your raspberry pi.

---

## 🚀 Features

- Airflow setup using Docker
- PostgreSQL as the metadata backend
- Persistent and reproducible configuration

---

## 🧱 Requirements

- A PostgreSQL instance (you can use the file `postgres/postgres.yaml` in this repository)

---

## 🛠️ Setup Guide

1. **Start PostgreSQL**

   Use the `postgres.yaml` file in this repository to launch a PostgreSQL instance.

2. **Create Airflow user and database**

   Access your PostgreSQL instance and run:

   ```sql
   CREATE USER airflow WITH PASSWORD 'airflow';
   CREATE DATABASE airflow OWNER airflow;

3. **Start Airflow** using the file `airflow.yaml`, once the Airflow container is running, reset the default admin password by entering the container shell:
```bash
docker exec -it <airflow_container_name>
airflow users reset-password --username admin
```

4. **Access Airflow**
Open your browser and go to:
```
http://<your-raspberry-pi-ip>:38080
```
Default login credentials:
- **Username**: `admin`
- **Password**: `yourpassword (the generated password in step 3)`

Make sure the environment variable `AIRFLOW__DATABASE__SQL_ALCHEMY_CONN` in `airflow.yaml` points to your PostgreSQL instance, for example: `postgresql+psycopg2://airflow:airflow@<postgres_ip>:<port>/airflow`

©️ Powered by [Apache Airflow](https://github.com/apache/airflow) 🛫 and Docker 🐳
