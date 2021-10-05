# Домашнее задание к занятию "4.1. Командная оболочка Bash: Практические навыки"

1. c = "a+b" - не содержит обращения к переменным с помощью $, поэтому a и b интерпретируются как строки.

    d = "1+3" - так как d не объявлена, как целочисленная переменная, то в данной конструкции операция будет проводиться со значениями переменных как с текстовыми строками.

    e = "3"   - такая конструкция выполняет арифметические операции со значениями переменных
2. Пропущена закрывающая скобка в while. Также желательно установить таймаут проверки, чтобы не сильно заполнять файл.
```bash
while (( 1 == 1 ))
do
  curl http://localhost:4757
  if (($? != 0))
  then
    echo $(date) - service unreachable  >> curl.log
  fi
  sleep 3
done
```
3. 
```bash
#!/bin/bash

hosts=(192.168.0.1 173.194.222.113 87.250.250.242)
timeout=3

for i in {1..5}
do
  for host in ${hosts[@]}
  do
    curl -Is --connect-timeout $timeout $host:80
    status=$?
    echo $(date) " - check" $host status=$status >> hosts.log
  done
done
```
4. 
```bash
#!/bin/bash

hosts=(192.168.0.1 173.194.222.113 87.250.250.242)
timeout=3
err_status=0

while (($err_status == 0))
do
  for host in ${hosts[@]}
  do
    curl -Is --connect-timeout $timeout $host:80
    err_status=$?
    if (($err_status != 0))
    then
         echo $(date) " - ERROR:" $host "is unreachable" >> err_hosts.log
         exit
    fi
  done
done
```