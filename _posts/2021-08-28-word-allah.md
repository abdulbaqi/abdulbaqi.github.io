---
layout: post
title: Mounting flask app on Nginx subdirectory
subtitle: A Quranic application
bigimg: /img/code.jpeg
image: /img/quran.jpeg
share-img: /img/quran.jpeg
tags: technology python quran
---
I have recently deployed a flask app on my DigitalOcean droplet. The app is simple and serves the verses of the quran based on URL link that specifes the sura no and verse number.

So, to display verse 20 from sura 10 for example, the link will be [http://www.abdulbaqi.site/quran/10/20](http://www.abdulbaqi.site/quran/10/20). 

 The app uses postgres database installed on the Ubuntu 20 droplet. I have used Nginx webserver. I have root served from the `abdulbaqi.site` but wanted to mount this flask app on a subdirectory on `abdulbaqi.site/quran`.

I will use another post to describe this app in detail, but in this post I wanted to keep a record of a problem I faced in deploying the app and how I solved it.

## The Problem of mounting flask app through Wsgi

The problem lies in the proper way of mounting the app on the WSGI specification, and the way to declare the block in the Nginx. 

Initially, I followd the [tutorial](https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-uwsgi-and-nginx-on-ubuntu-20-04) on DigitalOcean on how to serve a flask app using Nginx. All steps worked perfectly, but the tutorial assumes the flask app to be served from the root of the website. As such the following was the nginx block description in `/etc/nginx/sites-available/myproject`

```linux
server {
    listen 80;
    server_name your_domain www.your_domain;

    location / {
        include uwsgi_params;
        uwsgi_pass unix:/home/sammy/myproject/myproject.sock;
    }
}
```
and the wsgi initial file at `~/myproject/myproject.ini` was as follows:

```linux
[uwsgi]
module = wsgi:app

master = true
processes = 5

socket = myproject.sock
chmod-socket = 660
vacuum = true

die-on-term = true
```

The above settings did not work.

## What did NOT work?
I have made normal static files serving me from the root at `/var/www/abdulbaqi.site/html`, and 
attempted naively to just change the nginx block by adding the subdirectory:

```linux
server {
        listen 80;
        listen [::]:80;

        root /var/www/abdulbaqi.site/html;
        index index.html index.htm index.nginx-debian.html;

        server_name abdulbaqi.site www.abdulbaqi.site;

        location / {
                try_files $uri $uri/ =404;
        }

        location /quran {
                include uwsgi_params;
                uwsgi_pass unix:/home/baqi/baqiflask/baqiflask.sock;
        }
}
```
Then, I started to investigage the problem on tons of sites. [Some](https://stackoverflow.com/questions/18967441/add-a-prefix-to-all-flask-routes) talked about `Blueprints`, [others](https://dlukes.github.io/flask-wsgi-url-prefix.html) brought in as solution some complex configurations using `SCRIPT_NAME` and what nots, yet [others](https://github.com/zalando/connexion/issues/527) took the root of `proxy_pass`. These could have been solution for their setup, but it either did not work for me, or I could not follow their suggestions in my environment.

## The solution

It appeared to me that the solution lies in a good combination between the `.ini` file of UWSGI and the Nginx block description. There must be a `mount` and `callable` and `manage-script-name` in the uwsgi configuration. So, the variour was the [uwsgi](https://flask.palletsprojects.com/en/2.0.x/deploying/uwsgi/) page that was since 2010! But old is gold. I followed their suggestion and made the following configurations.

```linux

>> more baqiflask.ini 

[uwsgi]
mount = /quran=baqiflask:app 
module = wsgi:app
callable = app
manage-script-name = true
master = true
processes = 5

socket = baqiflask.sock
chmod-socket = 660
vacuum = true

die-on-term = true
```


```linux
>> more /etc/nginx/sites-available/abdulbaqi.site 


server {
        listen 80;
        listen [::]:80;

        root /var/www/abdulbaqi.site/html;
        index index.html index.htm index.nginx-debian.html;

        server_name abdulbaqi.site www.abdulbaqi.site;

        location / {
                try_files $uri $uri/ =404 @quran;
        }

	location @quran {
		include uwsgi_params;
		uwsgi_pass unix:/home/baqi/baqiflask/baqiflask.sock;
	}
}
```

I did not know that `@quran` is the way to refer to the subdirectory in nginx url system.