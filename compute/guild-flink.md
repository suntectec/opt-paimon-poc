```shell
echo "alias dflink='cd /opt/paimon-poc/compute && docker compose -f docker-compose-flink.yml '" >> ~/.bashrc
source ~/.bashrc
```

```shell
# To Access the SQL CLI, execute
docker exec -it sql-client bin/sql-client.sh embedded
docker compose run sql-client
```

```sql
-- 1
select 1
```