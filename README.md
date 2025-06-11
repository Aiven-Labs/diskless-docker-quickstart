# Inkless Kafka + MinIO Demo

Minimal setup using the official Aiven Inkless Kafka Docker image with MinIO object storage.

## Quick Start

```bash
# Build and start services
docker-compose up -d

# Check status (wait for inkless-kafka to be healthy)
docker-compose ps

# Create a new topic
docker-compose exec inkless-kafka kafka-topics.sh \  
--create --topic test-diskless \                                                                                                     
--bootstrap-server localhost:9092 \
--config inkless.enable=true

# Produce to the topic
echo "This should go to MinIO" | docker-compose exec -T kafka \
/opt/kafka/bin/kafka-console-producer.sh \
--topic test-diskless \
--bootstrap-server localhost:9092

# Consume from topic
docker-compose exec kafka /opt/kafka/bin/kafka-console-consumer.sh \
  --topic test-diskless --bootstrap-server localhost:9092 --from-beginning
```

Check out the segment files in the MinIO bucket by visiting [http://localhost:9001/](http://localhost:9001/) in your browser, username and password are `minioadmin` and `minioadmin` respectively.

Cleanup

```bash
# Stop any running containers
docker-compose down -v
```