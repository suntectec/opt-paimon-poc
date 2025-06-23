```shell
echo "alias dseatunnel='cd /opt/paimon-poc/compute && docker compose -f docker-compose-seatunnel.yml '" >> ~/.bashrc
source ~/.bashrc
```

```shell
Job Operation on cluster
use docker as a client
submit job :
# you need update yourself master container ip to `ST_DOCKER_MEMBER_LIST`
docker run --name seatunnel_client \
    --network seatunnel_seatunnel_network \
    -e ST_DOCKER_MEMBER_LIST=192.168.138.16:15801 \
    --rm \
    apache/seatunnel \
    ./bin/seatunnel.sh  -c config/v2.batch.config.template

list job
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

more command please refer user-command
```

```sql
-- 1
select 1
```