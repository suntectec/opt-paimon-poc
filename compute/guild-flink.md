```shell
echo "alias dflink='cd /opt/paimon-poc/compute && docker compose -f docker-compose-flink.yml '" >> ~/.bashrc
source ~/.bashrc
```

```shell
# Start the SQL Client run
docker compose run sql-client
```

```sql
-- 1
select 1
```