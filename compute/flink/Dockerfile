FROM flink:1.20
# Download connector libraries
RUN wget -P /opt/flink/lib/plugins https://repo1.maven.org/maven2/org/apache/paimon/paimon-flink-1.20/1.1.1/paimon-flink-1.20-1.1.1.jar; \
    wget -P /opt/flink/lib/plugins https://repo1.maven.org/maven2/org/apache/flink/flink-shaded-hadoop-2/2.8.3-10.0/flink-shaded-hadoop-2-2.8.3-10.0.jar;
