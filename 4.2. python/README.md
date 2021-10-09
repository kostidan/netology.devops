# Домашнее задание к занятию "4.2. Использование Python для решения типовых DevOps задач"

1. c = a + b - попытка работы с разными типамипеременных, так как a - целое, b - строка.

    c = str(a) + b  - с примет значение 12 

    c = a + int(b)  - c примет значение 3
2. Переменная is_changed не нужна, команда break прерывает работу скрипта на первом вхождении modified, из-за чего отображаются не все изменения. Также путь изменен на абсолютный.
```bash
#!/usr/bin/env python3

import os

bash_command = ["cd /home/devops/devops-netology", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
#is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(prepare_result)
        #break
```
3. 
```bash 
#!/usr/bin/env python3
import os
import sys
path = os.getcwd()

if len(sys.argv) >= 2:
    path = sys.argv[1]

bash_command = ["cd "+path, "git status 2>&1"]

result_os = os.popen(' && '.join(bash_command)).read()

for result in result_os.split('\n'):
    if result.find('fatal') != -1:
        print('Каталог '+path+' не является репозиторием git')
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(prepare_result)
```
4. 
```bash
#!/usr/bin/env python3

import socket
import time
import datetime

service = {'drive.google.com':'0.0.0.0', 'mail.google.com':'0.0.0.0', 'google.com':'0.0.0.0'}
wait = 2

for host in service:
    service[host] = socket.gethostbyname(host)

while 1==1 :
    for host in service:
        ip = socket.gethostbyname(host)
        if ip != service[host]:
            print(datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")+' [ERROR] ' + str(host) +' IP mistmatch: '+service[host]+' '+ip)
            service[host]=ip
    time.sleep(wait)
```