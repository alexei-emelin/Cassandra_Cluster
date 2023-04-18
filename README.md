# Создаем кластер из 3-х нод и обеспечиваем их доступность из локальной сети по отдельному ip адресу.

Структура кластера описана в docker-compose.yml

Скачиваем себе этот файл и запускаем:

```bash
docker compose up
``` 

Видим, что контейнеры запустились:

```bash
$ docker ps -a
CONTAINER ID   IMAGE                     COMMAND                  CREATED        STATUS        PORTS                                                 NAMES
edbfa2e5a9af   bitnami/cassandra:4.0.8   "/opt/bitnami/script…"   20 hours ago   Up 20 hours   7000/tcp, 0.0.0.0:9042->9042/tcp, :::9042->9042/tcp   cass_node1
1753bf3f03c6   bitnami/cassandra:4.0.8   "/opt/bitnami/script…"   20 hours ago   Up 20 hours   7000/tcp, 0.0.0.0:9044->9042/tcp, :::9044->9042/tcp   cass_node3
544b1e03d2b4   bitnami/cassandra:4.0.8   "/opt/bitnami/script…"   20 hours ago   Up 20 hours   7000/tcp, 0.0.0.0:9043->9042/tcp, :::9043->9042/tcp   cass_node2 
``` 

Заходим внутрь контейнера и проверяем связанность нод:

```bash
$ docker exec -it edbfa2e5a9af bash 
$ nodetool status
Datacenter: datacenter1
=======================
Status=Up/Down
|/ State=Normal/Leaving/Joining/Moving
--  Address     Load         Tokens  Owns (effective)  Host ID                               Rack 
UN  172.30.0.4  1 MiB        256     67.6%             23c21d58-c64e-445b-824d-eaa359f952b3  rack1
UN  172.30.0.2  1022.9 KiB   256     68.5%             df9b7e6d-6c2d-4b74-94c7-14112e77dccc  rack1
UN  172.30.0.3  1017.98 KiB  256     63.9%             1abed35b-fbc6-4e43-bf63-6173c4943b96  rack1
``` 