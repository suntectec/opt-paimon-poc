# Trying out Apache Paimon with Apache Flink using Docker Compose - Proof of Concept

Running Apache Flink containers using Docker Compose is a convenient way to get up and running to try out some Flink workloads.

Assuming docker compose is installed you can start the containers using the following command in the same folder as the docker compose file:

Operation Linux host location: /opt/paimon-poc

```
docker compose -f docker-compose-<service> up -d
```

This will start the containers in the background and you can check that the containers are running using

```
docker compose -f docker-compose-<service> ps
```
