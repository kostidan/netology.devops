
# Домашнее задание к занятию "5.3. Введение. Экосистема. Архитектура. Жизненный цикл Docker контейнера"


## Задача 1

https://hub.docker.com/r/kostidan/nginx

## Задача 2

*Высоконагруженное монолитное java веб-приложение*
- физический сервер. Монолитное, значит не реализуемо в микросерверах без изменения кода, высоконагруженное - значит 
  необходим физический доступ к ресурсам.

*Nodejs веб-приложение*
- Docker. Это веб-приложение, поэтому невысокая нагрузка и только программные зависимости.

*Мобильное приложение c версиями для Android и iOS*

- виртуальные машины на каждую ОС, так как для мобильных приложений требуется графический интерфейс.
 
*Шина данных на базе Apache Kafka*

- контейнерная виртуализация Docker с отказоустойчивостью на уровне кластера.

*Elasticsearch кластер для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два 
logstash и две ноды kibana*

- можно комбинировать Docker и виртуальные машины. Например Elasticsearch развернуть на ВМ, а logstash и kibana на 
  docker.

*Мониторинг-стек на базе Prometheus и Grafana*

- Docker. Невысокая нагруженность, быстрая развертываемость и масштабируемость.

*MongoDB, как основное хранилище данных для java-приложения*

- можно запустить в отдельном от приложения docker-контейнере. 

*Gitlab сервер для реализации CI/CD процессов и приватный (закрытый) Docker Registry*

- Docker. Полностью соответствует задаче.

## Задача 3

- Запустите первый контейнер из образа ***centos*** c любым тэгом в фоновом режиме, подключив папку ```/data``` из текущей рабочей директории на хостовой машине в ```/data``` контейнера;

```docker run -it --name centos -v /data:/data -d centos bash```

- Запустите второй контейнер из образа ***debian*** в фоновом режиме, подключив папку ```/data``` из текущей рабочей директории на хостовой машине в ```/data``` контейнера;

```docker run -it --name debian -v /data:/data -d debian:stretch-slim bash```

- Подключитесь к первому контейнеру с помощью ```docker exec``` и создайте текстовый файл любого содержания в ```/data```;

```
root@vagrant:~# docker exec -it centos bash
[root@49b7d45063ba /]# echo "Hello, world!" > /data/first.txt
[root@49b7d45063ba /]# cat /data/first.txt
Hello, world!
[root@49b7d45063ba /]# exit
exit
root@vagrant:~#
```

- Добавьте еще один файл в папку ```/data``` на хостовой машине;

```
root@vagrant:~# echo "Hello, world!" > /data/second.txt
root@vagrant:~# cat /data/second.txt
Hello, world!!!
root@vagrant:~#
```

- Подключитесь во второй контейнер и отобразите листинг и содержание файлов в ```/data``` контейнера.

```
root@vagrant:~# docker exec -it debian bash
root@ab086ef717a6:/# cd /data
root@ab086ef717a6:/data# ls
first.txt  second.txt
root@ab086ef717a6:/data# cat first.txt
Hello, world!
root@ab086ef717a6:/data# cat second.txt
Hello, world!!!
root@ab086ef717a6:/data#
```

## Задача 4 (*)

https://hub.docker.com/r/kostidan/ansible

