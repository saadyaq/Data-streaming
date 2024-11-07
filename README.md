Kafka-Spark-Cassandra Streaming Pipeline
Overview

This project implements a streaming data pipeline using Apache Kafka, Apache Spark, and Cassandra. The pipeline fetches user data from a public API, processes it, and stores it in a Cassandra database. Apache Airflow is used for orchestrating the data fetching and streaming tasks.
Components

    Apache Kafka: Used for streaming data between components.
    Apache Spark: Processes incoming Kafka streams and inserts data into Cassandra.
    Cassandra: A NoSQL database to store structured user data.
    Apache Airflow: Schedules and manages the streaming tasks.
    Docker: Containerizes the entire setup for easy deployment and scalability.

Prerequisites

To run this project, ensure you have:

    Docker and Docker Compose installed.
    Access to Kafka, Spark, Cassandra, and Airflow Docker images.
    Internet access for API requests.

Project Structure

    kafka_stream.py: Defines the DAG to fetch and stream data using Apache Airflow.
    spark_stream.py: Manages Spark jobs for processing Kafka streams and writing data to Cassandra.
    requirements.txt: Lists required Python packages for running the pipeline.
    docker-compose.yml: Defines the containerized environment for all services.

Setup and Installation

    Clone the repository:

git clone <repository_url>
cd <repository_name>

Install dependencies: Ensure Python dependencies are installed by running:

pip install -r requirements.txt

Run Docker Compose: Launch the containers:

    docker-compose up -d

    Configure Airflow:
        Place kafka_stream.py in the Airflow DAGs directory to start the scheduled data fetching.

Pipeline Workflow

    Data Fetching:
        The Airflow DAG in kafka_stream.py fetches data from the Random User API and formats it.
        It sends the formatted data to a Kafka topic (users_created).

    Data Processing (Spark):
        spark_stream.py reads messages from Kafka, processes the JSON data, and prepares it for insertion.
        Spark streams the processed data to the Cassandra database under the spark_streams keyspace.

    Data Storage (Cassandra):
        Data is stored in the created_users table in the spark_streams keyspace.
        Each record includes user information such as ID, name, gender, address, and contact details.

Configuration
Kafka

Kafka is configured to use a broker on localhost:29092. Ensure the topic users_created is created before streaming.
Spark

Spark is configured with the following packages:

    Cassandra Connector (com.datastax.spark:spark-cassandra-connector_2.13:3.4.1)
    Kafka Connector (org.apache.spark:spark-sql-kafka-0-10_2.13:3.4.1)

Cassandra

Cassandra configuration includes:

    Keyspace: spark_streams
    Table: created_users

Schema:

CREATE TABLE spark_streams.created_users (
    id UUID PRIMARY KEY,
    first_name TEXT,
    last_name TEXT,
    gender TEXT,
    address TEXT,
    post_code TEXT,
    email TEXT,
    username TEXT,
    registered_date TEXT,
    phone TEXT,
    picture TEXT
);

Running the Pipeline

    Start the Docker containers:

    docker-compose up

    Access Airflow and trigger the DAG:
        Go to http://localhost:8080 (or the configured Airflow URL).
        Trigger the user_automation DAG to begin fetching and streaming data.

    Monitor Spark and Cassandra logs to ensure data flow through the pipeline.

Troubleshooting

    Kafka Connection Issues: Check if Kafka brokers are up and accessible.
    Spark Errors: Confirm that Spark has access to Kafka and Cassandra.
    Cassandra Schema Errors: Ensure the keyspace and table are created before running the Spark job.

Acknowledgments

This project uses:

    Apache Kafka
    Apache Spark
    Apache Cassandra
    Apache Airflow
