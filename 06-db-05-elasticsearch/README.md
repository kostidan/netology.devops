# Домашнее задание к занятию "6.5. Elasticsearch"

## Задача 1

- текст Dockerfile манифеста  
[Dockerfile](assets/Dockerfile)  
[elasticsearch.yml](assets/elasticsearch.yml)
- ссылку на образ в репозитории dockerhub  
[https://hub.docker.com/r/kostidan/elasticsearch](https://hub.docker.com/r/kostidan/elasticsearch)
- ответ `elasticsearch` на запрос пути `/` в json виде  
[elasticsearch.json](assets/elasticsearch.json)

## Задача 2
```buildoutcfg
curl -X PUT localhost:9200/ind-1 -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 1,  "number_of_replicas": 0 }}'
curl -X PUT localhost:9200/ind-2 -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 2,  "number_of_replicas": 1 }}'
curl -X PUT localhost:9200/ind-3 -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 4,  "number_of_replicas": 2 }}'
```
*Получите список индексов и их статусов, используя API и **приведите в ответе** на задание.*
```buildoutcfg
root@vagrant:~# curl -X GET 'http://localhost:9200/_cat/indices?v'
health status index uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   ind-1 cn4bt1-kRvOV-NZnr7fMCw   1   0          0            0       208b           208b
yellow open   ind-3 CRnf5pZ5Ro6lY89jACN3qg   4   2          0            0       832b           832b
yellow open   ind-2 HThCloTCSsGyIODvonvCAA   2   1          0            0       416b           416b
root@vagrant:~# curl -X GET 'http://localhost:9200/_cluster/health/ind-1?pretty'
{
  "cluster_name" : "netology_test",
  "status" : "green",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 1,
  "active_shards" : 1,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 0,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 100.0
}
root@vagrant:~# curl -X GET 'http://localhost:9200/_cluster/health/ind-2?pretty'
{
  "cluster_name" : "netology_test",
  "status" : "yellow",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 2,
  "active_shards" : 2,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 2,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 41.17647058823529
}
root@vagrant:~# curl -X GET 'http://localhost:9200/_cluster/health/ind-3?pretty'
{
  "cluster_name" : "netology_test",
  "status" : "yellow",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 4,
  "active_shards" : 4,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 8,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 41.17647058823529
}

```
*Получите состояние кластера `elasticsearch`, используя API.*
```buildoutcfg
root@vagrant:~# curl -XGET localhost:9200/_cluster/health/?pretty=true
{
  "cluster_name" : "netology_test",
  "status" : "yellow",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 7,
  "active_shards" : 7,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 10,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 41.17647058823529
}
```
*Как вы думаете, почему часть индексов и кластер находится в состоянии yellow?*

Потому что кластер состоит из одиночного сервера, а для 2 и 3 индекса указано количество реплик, следовательно 
создавать реплики не на чем.  

*Удалите все индексы.*
```buildoutcfg
curl -X DELETE 'http://localhost:9200/ind-1?pretty'
curl -X DELETE 'http://localhost:9200/ind-2?pretty'
curl -X DELETE 'http://localhost:9200/ind-3?pretty'
```

## Задача 3

```buildoutcfg
root@vagrant:~# curl -XPOST localhost:9200/_snapshot/netology_backup?pretty -H 'Content-Type: application/json' -d'{"type": "fs", "settings": { "location":"/elasticsearch-7.11.1/snapshots" }}'
{
  "acknowledged" : true
}
```
```buildoutcfg
{
  "netology_backup" : {
    "type" : "fs",
    "settings" : {
      "location" : "/elasticsearch-7.11.1/snapshots"
    }
  }
}
```
*Создайте индекс `test` с 0 реплик и 1 шардом и **приведите в ответе** список индексов.*
```buildoutcfg
root@vagrant:~# curl -X PUT localhost:9200/test -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 1,  "number_of_replicas": 0 }}'
{"acknowledged":true,"shards_acknowledged":true,"index":"test"}
root@vagrant:~# curl -X GET 'http://localhost:9200/_cat/indices?v'
health status index uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   test  CmVAMTS8TMOp_OzaXZJ8cg   1   0          0            0       208b           208b
root@vagrant:~#
```
*[Создайте `snapshot`](https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshots-take-snapshot.html) 
состояния кластера `elasticsearch`.*
```buildoutcfg
root@vagrant:~# curl -X PUT localhost:9200/_snapshot/netology_backup/elasticsearch?wait_for_completion=true
{"snapshot":{"snapshot":"elasticsearch","uuid":"A99pDxviSXGXX6HEEb9QDw","version_id":7110199,"version":"7.11.1","indices":["test"],"data_streams":[],"include_global_state":true,"state":"SUCCESS","start_time":"2021-12-10T20:10:05.387Z","start_time_in_millis":1639167005387,"end_time":"2021-12-10T20:10:05.387Z","end_time_in_millis":1639167005387,"duration_in_millis":0,"failures":[],"shards":{"total":1,"failed":0,"successful":1}}}

```
***Приведите в ответе** список файлов в директории со `snapshot`ами.*
```buildoutcfg
[elasticsearch@74bdf549d03f snapshots]$ ls -la
total 60
drwxr-xr-x 1 elasticsearch elasticsearch  4096 Dec 10 20:10 .
drwxr-xr-x 1 elasticsearch elasticsearch  4096 Dec 10 18:48 ..
-rw-r--r-- 1 elasticsearch elasticsearch   437 Dec 10 20:10 index-0
-rw-r--r-- 1 elasticsearch elasticsearch     8 Dec 10 20:10 index.latest
drwxr-xr-x 3 elasticsearch elasticsearch  4096 Dec 10 20:10 indices
-rw-r--r-- 1 elasticsearch elasticsearch 30843 Dec 10 20:10 meta-A99pDxviSXGXX6HEEb9QDw.dat
-rw-r--r-- 1 elasticsearch elasticsearch   269 Dec 10 20:10 snap-A99pDxviSXGXX6HEEb9QDw.dat
```
*Удалите индекс `test` и создайте индекс `test-2`. **Приведите в ответе** список индексов.*
```buildoutcfg
root@vagrant:~# curl -X DELETE 'http://localhost:9200/test?pretty'
{
  "acknowledged" : true
}
root@vagrant:~# curl -X PUT localhost:9200/test-2?pretty -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 1,  "number_of_replicas": 0 }}'
{
  "acknowledged" : true,
  "shards_acknowledged" : true,
  "index" : "test-2"
}
root@vagrant:~# curl -X GET 'http://localhost:9200/_cat/indices?v'
health status index  uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   test-2 bVkXymmjTK64JHxTMQJPuQ   1   0          0            0       208b           208b
```
*[Восстановите](https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshots-restore-snapshot.html) 
состояние
кластера `elasticsearch` из `snapshot`, созданного ранее.*

***Приведите в ответе** запрос к API восстановления и итоговый список индексов.*
```buildoutcfg
root@vagrant:~# curl -X POST localhost:9200/_snapshot/netology_backup/elasticsearch/_restore?pretty -H 'Content-Type: application/json' -d'{"include_global_state":true}'
{
  "accepted" : true
}
root@vagrant:~# curl -X GET 'http://localhost:9200/_cat/indices?v'
health status index  uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   test-2 bVkXymmjTK64JHxTMQJPuQ   1   0          0            0       208b           208b
green  open   test   woh6m5RvQFKg6kR2cO-wEA   1   0          0            0       208b           208b
```