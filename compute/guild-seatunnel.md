# Seatunnel

优化执行命令 docker compose -f <> 起别名 d<>

```shell
# set alias in bashrc
echo "alias dseatunnel='cd /opt/paimon-poc/compute && docker compose -f docker-compose-seatunnel.yml '" >> ~/.bashrc
source ~/.bashrc
```

Docker作为客户端，提交作业到 Seatunnel Cluster 执行

```shell
# submit job :
# you need update yourself master container ip to `ST_DOCKER_MEMBER_LIST`
docker run --name seatunnel_client \
    --network seatunnel_seatunnel_network \
    -e ST_DOCKER_MEMBER_LIST=192.168.138.16:15801 \
    --rm \
    apache/seatunnel \
    ./bin/seatunnel.sh  -c config/v2.batch.config.template

# list job
# you need update yourself master container ip to `ST_DOCKER_MEMBER_LIST`
docker run --name seatunnel_client \
    --network seatunnel_seatunnel_network \
    -e ST_DOCKER_MEMBER_LIST=192.168.138.16:15801 \
    --rm \
    apache/seatunnel \
    ./bin/seatunnel.sh  -l

# submit job to cluster
docker run --name seatunnel_client \
    --network seatunnel_seatunnel_network \
    -e ST_DOCKER_MEMBER_LIST=seatunnel-master:5801 \
    -v ./seatunnel/config:/config \
    --rm \
    apache/seatunnel \
    ./bin/seatunnel.sh -c /config/sqlserver2paimon.stream.conf
```

直接执行 job 提交命令 或者 进入客户端依次执行

```shell
# new cluster version with running seatunnel-client container
# To Access the Seatunnel CLI, execute after seatunnel-client up
docker exec -it seatunnel-client ./bin/seatunnel.sh -c /config/sftpcsv2console.batch.conf
```

```shell
# old local version without running seatunnel-client container
docker exec -it seatunnel-test bash

# flink engine
bin/start-seatunnel-flink-15-connector-v2.sh -c /config/sqlserver2paimon.stream.conf

# zeta engine
bin/seatunnel.sh -m local -c /config/sqlserver2paimon.stream.conf
```

# Flink SQL Client

访问 Paimon

```sql
-- CATALOG
CREATE CATALOG paimon_catalog WITH (
  'type'='paimon',
  'warehouse'='s3a://warehouse/paimon/seatunnel/',
  's3.endpoint'='http://192.168.138.15:9000',
  's3.access-key'='minioadmin',
  's3.secret-key'='minioadmin',
  's3.path.style.access'='true'
);
       
USE CATALOG paimon_catalog;

USE paimon;

select count(*) from order_protobuf_format;
select * from order_protobuf_format;

-- org.apache.flink.table.api.TableException: Column 'id' is NOT NULL, however, a null value is being written into it. You can set job configuration 'table.exec.sink.not-null-enforcer'='DROP' to suppress this exception and drop such records silently.
set 'table.exec.sink.not-null-enforcer'='DROP';


SET 'execution.checkpointing.interval' = '5 s';

-- switch to streaming mode
SET 'execution.runtime-mode' = 'streaming';
-- use tableau result mode
SET 'sql-client.execution.result-mode' = 'tableau';

-- switch to batch mode
RESET 'execution.checkpointing.interval';
SET 'execution.runtime-mode' = 'batch';

SELECT id,order_id,supplier_id,item_id,qty FROM seatunnel_sqlserver_paimon_sink where id = 247048;
```