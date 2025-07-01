# Trying out Apache Paimon, Flink and SeanTunnel within Docker Container - Proof of Concept

![The Apache Software Foundation](https://apache.org/img/asf-estd-1999-logo.jpg)

![Static Badge](https://img.shields.io/badge/Apache-Paimon-blue?logo=apache&logoColor=%23E6526F&labelColor=black)
![Static Badge](https://img.shields.io/badge/Apache-Flink-blue?logo=apache&logoColor=%23E6526F&labelColor=black)
![Static Badge](https://img.shields.io/badge/Apache-SeaTunnel-blue?logo=apache&logoColor=%23E6526F&labelColor=black)

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
