FROM openjdk:11

ARG VERSION
ENV SEATUNNEL_HOME=/opt/seatunnel

# Download From Internet
# Please Note this file only include fake/console connector, You'll need to download the other connectors manually
RUN cd /opt && \
    wget -P /opt https://dlcdn.apache.org/seatunnel/${VERSION}/apache-seatunnel-${VERSION}-bin.tar.gz && \
    tar -zxvf apache-seatunnel-${VERSION}-bin.tar.gz && \
    mv apache-seatunnel-${VERSION} seatunnel && \
    rm apache-seatunnel-${VERSION}-bin.tar.gz && \
    sed -i 's/#rootLogger.appenderRef.consoleStdout.ref/rootLogger.appenderRef.consoleStdout.ref/' seatunnel/config/log4j2.properties && \
    sed -i 's/#rootLogger.appenderRef.consoleStderr.ref/rootLogger.appenderRef.consoleStderr.ref/' seatunnel/config/log4j2.properties && \
    sed -i 's/rootLogger.appenderRef.file.ref/#rootLogger.appenderRef.file.ref/' seatunnel/config/log4j2.properties && \
    cp seatunnel/config/hazelcast-master.yaml seatunnel/config/hazelcast-worker.yaml

COPY seatunnel/install-plugin.sh ${SEATUNNEL_HOME}/bin/install-plugin.sh

COPY seatunnel/plugin_config ${SEATUNNEL_HOME}/config/plugin_config

WORKDIR ${SEATUNNEL_HOME}