---
title:      "letsencrypt"
date:       2023-10-30T16:33:13-04:00
tags:       ["lit", "ssl", "tech", "tsl"]
identifier: "20231030T163313"
---


you use certbot with letsencrypt

letsencrypt is a nonprofit Certificate Authority that provides TLS certificates

certbot is a free opensource software tool for automaticaly using letsencrypt certificates

certbot
* certbot communicates with lets encrypt
* letsencrypt asked to put a signature in a special place so that the letsencrpt service can verify that we own the domain

instructions to setup ubuntu
https://certbot.eff.org/instructions?ws=nginx&os=ubuntufocal
- it's crazy easy

1. install certbot with snapd
`sudo snap install --classic certbot`

2. next ask certbot to config your nginx
`sudo certbot --nginx`
- it automatically sets up a cron job for you

certbot communicates with letsencrypt
* letsencrypt ask to host a file on the server to verify that you are inded the owner of the website


where we host our verifcation file from letsencrypt
``` conf
location /.well-known/acme-challenge/ {
  root /letsencrypt/;
}
```

start the verification process

``` shell
certbot certonly --webroot
```
- it will ask for your email, domain and where you want to store the letsencrypt challange file
  * this is also where you certificate will be stored
  
  
to use the certificate

``` conf
  server {
    listen 443 ssl default_server;
	listen [::]:443 ssl defalut_server;
	server_name diego.vila;
	ssl_certificate /etc/letsencrypt/live/diego.vila/fullchain.pem;
	ssl_certificate_key /etc/letsencrypt/live/diego.vila/privkey.pem;
	
	root <path/to/folder>
	
	location / {
	  gzip off;
	  root <path/to/folder>
	  index index.html
	}
  }
```

Renew
=====

you can renew with a cron job

`certbot renew`

sometime you need to restart your server



