---
title: Guide - How to set up a nodejs application on ubuntu 18?
---

## Introduction

This guide is a recollection of knowledge shared by others, this is intended to make the guide easier for myself: 


The following frameworks, technologies, and services were used:
* node 14.16.1
* digital ocean
* ubuntu 18


You will need to have the following prepared:
* A domain name pointed at your server’s public IP

<!--more-->

## Install nvm

[nvm installment](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-18-04)

1. Source the .bashrc file

```shell
source ~/.bashrc
```


2. This installs the nvm script to your user account.

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash

```

4. List all node versions
```shell
nvm ls-remote
```

6. Install a version of node:

```bash
nvm install v16.13.1
```

7. Create symlinks for npm and node
```shell
sudo rm -f /usr/bin/node
sudo rm -f /usr/bin/npm
sudo ln -s $(which node) /usr/bin/
sudo ln -s $(which npm) /usr/bin/
```


<br />

## Install Nginx

1. Refresh our local package index first

```bash
sudo apt-get update
```

2. Install from the repositories

```bash
sudo apt-get install nginx
```

3. Adjust the firewall
```bash
sudo ufw app list
```

4. Check status
```bash
sudo ufw status
```

5. Allow https traffic
```bash
sudo ufw allow 'Nginx Full'
```

6. Check your web server

```bash
systemctl status nginx
```

Some useful commands 
* To stop your web server
```bash
sudo systemctl stop nginx
```

* To start your web server
```bash
sudo systemctl start nginx
```

* To stop and start your web server
```bash
sudo systemctl restart nginx
```

* If you are simply making configuration changes,
  Nginx can often reload without dropping connections
```bash
sudo systemctl reload nginx
```

<br />

## Install certbot
[How To Secure Nginx with Let's Encrypt on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-16-04)

1. Add repository
```bash
sudo add-apt-repository ppa:certbot/certbot
```

2. Update package information
```bash
sudo apt-get update
```

3. Install certbot nginx package
```bash
sudo apt-get install python-certbot-nginx
```

4. Setup nginx
*Certbot can automatically configure SSL for Nginx, but it needs
   to be able to find the correct server block in your config. 
   It does this by looking for a server_name directive that matches
   the domain you’re requesting a certificate for.*
   
```bash
sudo nano /etc/nginx/sites-available/default
```

5. Check for mistakes on nginx config
```bash
sudo nginx -t
```

6. Reload nginx config
```bash
sudo systemctl reload nginx
```

7. Obtaining a ssl certificate. 'example.com' and 'www.example.com' should be substituted for
your website
```bash
sudo certbot --nginx -d example.com -d www.example.com
```
```
Output
Please choose whether or not to redirect HTTP traffic to HTTPS, removing HTTP access.
-------------------------------------------------------------------------------
1: No redirect - Make no further changes to the webserver configuration.
2: Redirect - Make all requests redirect to secure HTTPS access. Choose this for
new sites, or if you're confident your site works on HTTPS. You can undo this
change by editing your web server's configuration.
-------------------------------------------------------------------------------
Select the appropriate number [1-2] then [enter] (press 'c' to cancel):
`
*redirect makes changes to the 'default' file in nginx config*
```

8. Verifying Certbot Auto-Renewal
```bash
sudo certbot renew --dry-run
```
*If you see no errors, you’re all set. When necessary, Certbot will
renew your certificates and reload Nginx to pick up the changes.
If the automated renewal process ever fails, Let’s Encrypt will
send a message to the email you specified, warning you when your certificate is about to expire.*


## Set up nginx as a reverse proxy server
1. Access /etc/nginx/sites-available/default
```bash
sudo nano /etc/nginx/sites-available/default
```

2. Configure file to make it a proxy server
```nginx
}
    location / {
        proxy_pass http://localhost:8080;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```
*This configures the server to respond to requests at its root.
Assuming our server is available at example.com, accessing
https://example.com/ via a web browser would send the request
to hello.js, listening on port 8080 at localhost.*

3. Check configuration changes
```bash
sudo nginx -t
```

4. Restart nginx
```bash
sudo systemctl restart nginx
```

<br />

## OPTIONAL

### Mysql config

[How do I set the time zone of MySQL? (stack overflow)](https://stackoverflow.com/questions/930900/how-do-i-set-the-time-zone-of-mysql)

Because of how mysql and typeorm change to a date native object. Dates strings get converted into a 
timezone date string and data may be incorrect. Some other precautions may be needed like changing how typeorm
store and retrieve DATE_TIME values. (see nestjs snippets)

In this case, we are setting the timezone to the one Mexico uses.

```d
[mysqld]
!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mysql.conf.d/
default-time-zone = "-05:00"
```

<br />

Restart mysql service


### Services

```shell
sudo service mysql restart
```

<br />

```bash
[Unit]
Description=Grupo inopack server application
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
Alias=inopack.service
```


### Postgres


