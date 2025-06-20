```shell
echo "alias dflink='cd /opt/paimon-poc/compute && docker compose -f docker-compose-seatunnel.yml '" >> ~/.bashrc
source ~/.bashrc
```

```shell
Job Operation on cluster
use docker as a client
submit job :
# you need update yourself master container ip to `ST_DOCKER_MEMBER_LIST`
docker run --name seatunnel_client \
    --network paimon-poc_seatunnel_network \
    -e ST_DOCKER_MEMBER_LIST=192.168.138.15:15801 \
    --rm \
    apache/seatunnel \
    ./bin/seatunnel.sh  -c config/v2.batch.config.template

list job
# you need update yourself master container ip to `ST_DOCKER_MEMBER_LIST`
docker run --name seatunnel_client \
    --network paimon-poc_seatunnel_network \
    -e ST_DOCKER_MEMBER_LIST=192.168.138.15:15801 \
    --rm \
    apache/seatunnel \
    ./bin/seatunnel.sh  -l

more command please refer user-command
```

```sql
-- 1
select 1
```