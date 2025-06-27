# Minio

优化执行命令 docker compose -f <> 起别名 d<>

```shell
# set alias in bashrc
echo "alias dminio='cd /opt/paimon-poc/data && docker compose -f docker-compose-minio.yml '" >> ~/.bashrc
source ~/.bashrc
```