# Домашнее задание к занятию "4.3. Языки разметки JSON и YAML"

1. 
```bash
{
    "info" : "Sample JSON output from our service",
    "elements" :[
        {
            "name" : "first",
            "type" : "server",
            "ip" : "71.75.1.1"
        },
        {
            "name" : "second",
            "type" : "proxy",
            "ip" : "71.78.22.43"
        }
    ]
}
```
2.
```bash
#!/usr/bin/env python3

import socket
import time
import datetime
import json
import yaml

service = {'drive.google.com':'0.0.0.0', 'mail.google.com':'0.0.0.0', 'google.com':'0.0.0.0'}
wait = 2

for host in service:
    service[host] = socket.gethostbyname(host)
def srv():
    with open('srv.json', 'w') as js:
        js.write(json.dumps(service, indent=2))
    with open('srv.yml', 'w') as ym:
        ym.write(yaml.dump(service, indent=2, explicit_start=True, explicit_end=True))

while 1==1 :
    for host in service:
        ip = socket.gethostbyname(host)
        if ip != service[host]:
            print(datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")+' [ERROR] ' + str(host) +' IP mistmatch: '+service[host]+' '+ip)
            service[host]=ip
    srv()
    time.sleep(wait)
```