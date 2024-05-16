---
title:      "nginx"
date:       2023-10-30T15:01:27-04:00
tags:       ["lit", "tech", "web"]
identifier: "20231030T150127"
---

gateway between your web browser and backend

nginx.conf
``` conf
user <username>;  # these are directives
error_log <path/to/error.log>;

# these are called context for directive
http {

  # media types you can handle
  types {
    text/css   css;
	text/html  html;
  }
  # or you can just include a file for all the mime types
  include mime.types;
  
  
  server {  # each server is assigned a specific port
    listen 80;
	access_log <path/to/access.log>;
	
	location / {
	  root <path/to/static-files>;
	}
	
	location ~ \.(gif|jpg|png)$ {
	  root <path/to/image>;
	}
	
	# redirect to other server
	location / {
	  proxy_pass http://localhost:5000;
	}
	
	# this appends fruits to root
	location /fruits {
	  root <path/to/append>
	}
	
	location /vegetables {
	  root <path/to/append>
	  # try these files
	  try_files /vegetables/veggies.html /index.html =404;
	}
	
	# rewrite number to count
	rewrite ^/number/(\w+) /count/$1;
	
	# regex
	location ~* /count/[0-9] {
	  root <path/to/dir>
	  try_files /index.html =404;
	}
	
	# this is just an allias
	location /carbs {  
	  alias <allias/to/path>
	}
	
	location /crops {
	  return 307 /fruits
	}
  }
  
  server {
    listen 5000;
	root <path/to/www>;
	
	location / {
	
	}
  }
}



```

server is like apache virtual host.
maps to an ip, domain, or sub-domain
it is also called a server block
``` conf
server {
  listen 80;
  listen [::]:80;
  server_name ruhaman.net;
  root /to/my/website/;
  index index.html;
  
  location / {
    # first try it as a file, then directory, then return 404
    try_files $uri $uri/ =404;
  }
}
```

`ln -s /etc/nginx/sites-available/mysite /etc/nginx/sites-enabled/`

`nginx -t`
- check if config is good

you can have multiple servers

``` conf
server {
  listen 80;
  server_name dev.nginx;
  location / {
    root /path/to/website/;
	index index.html;
  }
}

server {
  listen 80;
  server_name newapp.nginx;
  
  location / {
    root /path/to/website/;
	index index.html;
  }
}
```

you can setup a default server
there can only be one.
default site incase there are none
