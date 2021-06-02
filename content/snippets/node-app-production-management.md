---
title: Node process management
---

# Introduction

systemd is a suite of basic building blocks for a Linux system. 
It provides a system and service manager that
runs as PID 1 and starts the rest of the system.
[Systemd.io](https://systemd.io/)


<br />

## Setup

Go to folder `/lib/systemd/system` and create a file with `.service` termination

Example: example.service

<br />

Basi setup

example.service
```bash
[Unit]
Description=Example service
After=network.target

[Service]
Restart=always
AmbientCapabilities=CAP_SYS_RAWIO
User=root
Type=simple
WorkingDirectory=/root/nestjs-inopack
Environment=NODE_PORT=3000
ExecStart=/usr/bin/npm run start:prod --prefix /root/nestjs-inopack


[Install]
WantedBy=multi-user.target
Alias=example.service
```

<br />

Enable the service

```shell
systemctl enable example.service
```

<br />

Reload daemon

```shell
systemctl daemon-reload
```

<br />


Start service
```shell
systemctl start example.service
```

<br />

## Logging

Last loggs of services

```shell
journalctl -f
```

<br />

Log specific service

```shell
journalctl -u example.service
```


## Monitoring

Enabled services
```shell
systemctl list-unit-files | grep enabled
```

<br />


Running services
```shell
systemctl | grep running
```


<br />

Real time processes view
```shell
top 
```


<br />

## Cleaning (kill processes)

```shell
killall node
```


