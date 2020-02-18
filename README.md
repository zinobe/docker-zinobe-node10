# Node 10

Node 10 with supervisord.

**Base image:** `node:10-slim`

**WORKDIR:** `/www`

Add **aws-env** util form AWS SSM ([aws-env](https://github.com/Droplr/aws-env/))

# Supervisor - Daemon config
```conf
[unix_http_server]
file = /run/supervisord.sock
chmod = 0760
chown = www:www

[supervisord]
pidfile=/run/supervisord.pid
; Log information is already being sent to /dev/stdout by default, which gets captured by Docker logs.
; Storing log information inside the contaner will be redundant, hence using /dev/null here
logfile = /dev/null
logfile_maxbytes = 0

[rpcinterface:supervisor]
supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface

[supervisorctl]
serverurl = unix:///run/supervisord.sock
```

# Docker compose (example)

docker-compose.yml
```yml
version: '3'

services:
  app:
    image: zinobe/node10
    volumes:
      - ./:/www
      - ./supervisord.conf:/etc/supervisord.conf:ro
      - ./entrypoint:/entrypoint:ro
    env_file:
      - /.env
    command: /entrypoint
```