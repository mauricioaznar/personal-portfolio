---
title: Linux
---

## Systemd app management

systemd is a suite of basic building blocks for a Linux system. It provides a system and service manager that runs as
PID 1 and starts the rest of the system.
[Systemd.io](https://systemd.io/)


<br />

### Setup

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

### Logging

Last loggs of services

```shell
journalctl -f
```

<br />

Log specific service

```shell
journalctl -u example.service
```

### Monitoring

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

* shift + m = order by memory usage

```shell
top 
```

<br />

Count the number of files in one directory

```shell
ls -1 | wc -l
```

<br />

Disk usage (human readable form)

```shell
du -sh file_path
```

<br />

### Cleaning (kill processes)

```shell
killall node
```

<br />

## User administration

<br />

add user

```shell
adduser sammy
```

<br />


add user to sudo group

* -a append (without this option, the current groups a user is linked to would be replaced by sudo)
* -G tells the `usermod` to changes a user's group settings

```shell
usermod -aG sudo sammy
```

<br />

copy the root user’s .ssh directory, preserve the permissions, and modify the file owners

```shell
rsync --archive --chown=sammy:sammy ~/.ssh /home/sammy
```

<br />

change user password as root

```shell
passwd sammy
```

<br />

## security

### Disabling password authentication

* change `PasswordAuthentication yes` to `PasswordAuthentication no`

```shell
sudo nano /etc/ssh/sshd_config
```

<br />

* restart
```shell
sudo systemctl restart ssh
```

<br />

### Disabling root ssh

* change `PermitRootLogin yes` to `PermitRootLogin no`

```shell
sudo nano /etc/ssh/sshd_config
```

<br />

* restart
```shell
sudo systemctl restart ssh
```

<br />

### Firewall

<br />

* UFW (Uncomplicated Firewall) is a firewall configuration tool that comes with Ubuntu servers. You can use the UFW
  firewall to make sure only connections to certain services are allowed on your server.
* 

<br />

get all list of all current available profiles

```shell
ufw app list
```

<br />

status

```shell
ufw status
```

<br />

enable ssh

```shell
ufw enable ssh
```

<br />

enable https

```shell
ufw enable https
```
