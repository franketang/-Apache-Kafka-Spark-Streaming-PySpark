# -Apache-Kafka-Spark-Streaming-PySpark

# Real-time Data Processing with Apache Kafka, Spark Streaming, and PySpark: A Practical Guide

Welcome to the "Real-time Data Processing with Apache Kafka, Spark Streaming, and PySpark" project. This comprehensive guide will walk you through setting up a real-time data processing pipeline that harnesses the power of Apache Kafka, Spark Streaming, and PySpark. By integrating these technologies, you'll be able to efficiently process streaming data and gain valuable insights in real time.

![Real-time Data Processing](http://www.externalharddrive.com/graphics/bullets/square/redsquare.gif)


![Google Slides Link](https://docs.google.com/presentation/d/1rZj799Y0V02H08tiTbXjI12VOqh1zzWDlHymkubUolo/edit?usp=sharing)
## Design

### Architecture Overview
The architecture of this project comprises the following key components:

- **Apache Kafka**: A robust distributed event streaming platform for ingesting and distributing data streams.
- **Spark Streaming**: A micro-batch processing framework built on Apache Spark for real-time data stream processing.
- **PySpark**: The Python API for Apache Spark, enabling you to build data processing applications using Python.
- **Data Pipeline**: Kafka acts as a data source for Spark Streaming, which processes data in mini-batches and performs analytics.
- **Output**: Processed data can be sent to various sinks for storage, visualization, or further analysis.

## Implementation (Kafka Example)

### Download and Extract Kafka
```bash
# Connect to your Dataproc cluster using SSH.
wget https://downloads.apache.org/kafka/3.5.1/kafka_2.12-3.5.1.tgz
tar -xvf kafka_2.12-3.5.1.tgz
```

### Install Dependencies
```bash
# Install required Python packages.
pip3 install msgpack kafka-python
```

### Start Zookeeper and Kafka Broker
```bash
# Start Zookeeper
cd kafka_2.12-3.5.1/
bin/zookeeper-server-start.sh config/zookeeper.properties

# Start Kafka broker in a new terminal.
cd kafka_2.12-3.5.1/
bin/kafka-server-start.sh config/server.properties
```

### Create Kafka Topic
```bash
cd kafka_2.12-3.5.1/
# Create a Kafka topic in a new terminal.
bin/kafka-topics.sh --create --topic input_event --bootstrap-server localhost:9092 --partitions 3 --replication-factor 1
```

### Create and Run Kafka Consumer
- Create a Python script in a new terminal named `consumer.py` and implement the consumer logic using the kafka-python library.

```python
from kafka import KafkaConsumer

consumer = KafkaConsumer('input_event', bootstrap_servers=['localhost:9092'])
for msg in consumer:
    print(msg)
```

- Run the consumer script:
```bash
python3 consumer.py
```

### Create and Run Kafka Producer
- Create a Python script in a new terminal named `producer.py` and implement the producer logic using the kafka-python library.

```python
from kafka import KafkaProducer

producer = KafkaProducer(bootstrap_servers='localhost:9092', value_serializer=str.encode, key_serializer=str.encode)
event_stream_key = 'product_list'
event_stream_value = 'product1 product2 product3 product1'
producer.send('input_event', key=event_stream_key, value=event_stream_value)
```

- Run the producer script:
```bash
python3 producer.py
```

### Test (Kafka Example)
- Monitor the consumer terminal to see messages consumed from the Kafka topic.
- Verify that messages sent by the producer are successfully received and printed by the consumer.

## Implementation (Kafka + Spark Streaming + PySpark)

### Start Zookeeper and Kafka Broker (Same as in Kafka Example)

### Create Kafka Topics
```bash
cd kafka_2.12-3.5.1/
# Create Kafka topics in a new terminal.
bin/kafka-topics.sh --create --topic input_event --bootstrap-server localhost:9092 --partitions 3 --replication-factor 1
bin/kafka-topics.sh --create --topic output_event --bootstrap-server localhost:9092 --partitions 3 --replication-factor 1
```

### Create and Run Kafka Consumer (Same as in Kafka Example)
- Create a Python script in a new terminal named `consumer.py` and implement the consumer logic using the kafka-python library.

### Create and Run Kafka Producer (Same as in Kafka Example)
- Create a Python script in a new terminal named `producer.py` and implement the producer logic using the kafka-python library.

### Run Spark Processor
- Create a Python script in a new terminal named `spark_processor.py`, and then submit the Spark job:

```bash
spark-submit --packages org.apache.spark:spark-streaming-kafka-0-10_2.12:3.1.3,org.apache.spark:spark-sql-kafka-0-10_2.12:3.1.3 --deploy-mode client spark_processor.py
```

## Test (Kafka + Spark Streaming + PySpark)

**Note:** The output may not match expectations as you're still working on it.

Feel free to use this guide to build your real-time data processing pipeline. Happy coding!
