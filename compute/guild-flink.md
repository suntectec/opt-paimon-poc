# Seatunnel

优化执行命令 docker compose -f <> 起别名 d<>

```shell
# set alias in bashrc
echo "alias dflink='cd /opt/paimon-poc/compute && docker compose -f docker-compose-flink.yml '" >> ~/.bashrc
source ~/.bashrc
```

直接执行 job 提交命令 或者 进入客户端依次执行

```shell
# To Submit Job Executing Directly
docker exec -it jobmanager /opt/flink/bin/sql-client.sh embedded -f /opt/flink/job.sql

# Or, To Access the SQL CLI, execute
docker exec -it sql-client bin/sql-client.sh embedded
```

# Flink SQL Client

访问 Paimon

```sql
DROP CATALOG paimon_catalog;

CREATE CATALOG paimon_catalog WITH (
  'type'='paimon',
  'warehouse'='s3://paimon/',
  's3.endpoint'='http://192.168.138.15:9900',
  's3.access-key'='minioadmin',
  's3.secret-key'='minioadmin',
  's3.path.style.access'='true'
);
       
USE CATALOG paimon_catalog;

CREATE DATABASE IF NOT EXISTS inventory;

USE inventory;

-- register a SqlServer table in Flink SQL
CREATE TEMPORARY TABLE sqlserver_source (
    id BIGINT,
    order_id VARCHAR(36),
    supplier_id INT,
    item_id INT,
    status VARCHAR(20),
    qty INT,
    net_price INT,
    issued_at TIMESTAMP,
    completed_at TIMESTAMP,
    spec VARCHAR(1024),
    created_at TIMESTAMP,
    updated_at TIMESTAMP,
    PRIMARY KEY (id) NOT ENFORCED
) WITH (
    'connector' = 'sqlserver-cdc',
    'hostname' = 'dev-ds-trm01.tailb6e5ab.ts.net',
    'port' = '1433',
    'username' = 'sa',
    'password' = 'StrongPassword123!',
    'database-name' = 'inventory',
    'table-name' = 'INV.orders'
);

SELECT * FROM sqlserver_source;

SELECT count(*) FROM sqlserver_source;

CREATE TABLE IF NOT EXISTS orders (
                                      id BIGINT,
                                      order_id VARCHAR(36),
    supplier_id INT,
    item_id INT,
    status VARCHAR(20),
    qty INT,
    net_price INT,
    issued_at TIMESTAMP,
    completed_at TIMESTAMP,
    spec VARCHAR(1024),
    created_at TIMESTAMP,
    updated_at TIMESTAMP,
    PRIMARY KEY (id) NOT ENFORCED
    );

-- required set before submit insert job, otherwise data not observe
-- execution.checkpointing.interval: default - none, The base interval setting. To enable checkpointing, you need to set this value larger than 0.
SET 'execution.checkpointing.interval' = '5 s';

INSERT INTO orders SELECT * FROM sqlserver_source;

SELECT * FROM orders;
SELECT count(*) FROM orders;
SELECT * FROM orders order by created_at desc;
SELECT id,order_id,supplier_id,qty FROM orders where id in (1,40);

-- switch to streaming mode
SET 'execution.runtime-mode' = 'streaming';
-- switch to batch mode
SET 'execution.runtime-mode' = 'batch';
-- use tableau result mode
SET 'sql-client.execution.result-mode' = 'TABLE';
SET 'sql-client.execution.result-mode' = 'CHANGELOG';
SET 'sql-client.execution.result-mode' = 'tableau';
-- or RESET
RESET 'execution.checkpointing.interval';

-- track the changes of table and calculate the count interval statistics
SELECT `status`, SUM(qty) AS qty_total FROM orders GROUP BY `status`;


-- Way 2 - Not Catalog
CREATE TABLE sqlserver_source (
                                  id INT,
                                  name VARCHAR(50),
                                  PRIMARY KEY (id) NOT ENFORCED
) WITH (
      'connector' = 'sqlserver-cdc',
      'hostname' = 'dev-ds-trm01.tailb6e5ab.ts.net',
      'port' = '1433',
      'username' = 'flink',
      'password' = 'flink',
      'database-name' = 'poc_db',
      'table-name' = 'lab.users'
      );

CREATE TABLE orders (
                        id INT,
                        name VARCHAR(50),
                        PRIMARY KEY (id) NOT ENFORCED
) WITH (
      'connector' = 'paimon',
      'path' = 'file:///tmp/paimon/warehouse',
      'auto-create' = 'true'
      );

SET 'execution.checkpointing.interval' = '5 s';

INSERT INTO orders SELECT * FROM sqlserver_source;

-- read
SELECT * FROM sqlserver_source;
SELECT * FROM orders;
```