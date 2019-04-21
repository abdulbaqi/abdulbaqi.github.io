---
layout: post
title: Configuring Nginx
subtitle: to run static, node and react contents
bigimg: /img/code.jpeg
image: /img/coding-sq.jpg
share-img: /img/coding-sq.jpg
tags: technology nginx
---
I found bits and pieces information here and there, and hence documenting the steps I followed as a future reference for me, and for anyone else in my shoes.

## Nginx setup
I created a droplet from DigitalOceal ($5/month) and followed their tutorial on installing NginX server on Ubuntu 18.04, as well as securing it with certbot.

During the process, I registered a domain from GoDaddy and created A records attaching that domain name to my droplet from my DigitalOcean dashboard.

The product is that when I point to my domain, I see an https link displaying the default nginx page indicating that everything went on fine.

Here are a few points to note in this process.

Understanding nginx server block directives and context is very important. I am still struggling with that. (I am referring to whatever block and files you create under /etc/nginx/sites-available/ folder). Note that when you get your registered domain certified by the certbot, it add’s few directives in the file related to your domain (in my case it is playquran.com)

```
#this is file resided at /etc/nginx/sites-available/playquran.com
server {
...
listen [::]:443 ssl ipv6only=on; # managed by Certbot
    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/playquran.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/playquran.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
...
}
```

Also note that (if you followed the digitialOcean tutorial you will know that) the static contents are available in the directory: var/www/playquran.com/html

### Node js
Once you are done with serving static files, you may want to serve some nodejs contents listening over some port. For this digitalocean also has a good tutorial. Follow that tutorial and have the following node file serving at port (whatever, but in my case) 4003, as follows:

```
const http = require('http');
const hostname = 'localhost';
const port = 4003;
const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World!\n');
});
server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

The tutorial will tell you to have pm2 to keep your app running in the background.

Now, you need to adjust your nginx server block to reflect this content as a location context as follows:

```
server {
...
location /nodejs {
        proxy_pass http://localhost:4003;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
...
}
```

Test this modification syntax by sudo nginx -t and then reload the nginx by sudo systemctl restart nginx and then point to https://yourdomain.com/nodejs to see the Hello World! displaying.

### Express
I have the following express app as app.js:

```
onst express = require('express');
const app = express();
const PORT = process.env.PORT || 4001;
app.listen(PORT, () => {
  console.log(`Server is listening on port ${PORT}`);
});
app.get('/', (req,res,next)=>{
 res.send('serinv your exprsso!');
});
```

and here is the location block:

```
location /express/ {
                proxy_pass http://localhost:4001/;
}
```

I have struggled a lot, and it appears that others as well because of the absence of the trailing / at the end of /express/ and the uri as well. This link was very helpful in resolving that issue.

### React
Now, let us turn our attention to a front-end react app. Let us assume you have your app available in a GitHub repo.

step 1. clone the app

step 2. cd to the directory

step 3. `npm install`

step 4. modify the `package.json` file to create an entry where you want the app to be available in my case it will be the ayaat subdirectory.

```
"homepage": "https://playquran.com/ayaat",
```
step 5. `npm run build` this will make npm follow the instruction in the package.json file and create static contents in the folder build

step 6. copy all files (and folders cp -r ) from the build folder into a folder (I will call it ayaat) inside your `/var/www/playquran.com/html` folder.

```
/var/www/playquran.com/html
├── ayaat
│   ├── asset-manifest.json
│   ├── favicon.ico
│   ├── index.html
│   ├── manifest.json
│   ├── precache-manifest.85fa01ead59977e4e253ccd9aafda3ed.js
│   ├── service-worker.js
│   └── static
│       ├── css
│       │   ├── main.71e49a20.chunk.css
│       │   └── main.71e49a20.chunk.css.map
│       ├── js
│       │   ├── 2.5dbd8ed4.chunk.js
│       │   ├── 2.5dbd8ed4.chunk.js.map
│       │   ├── main.f41582ff.chunk.js
│       │   ├── main.f41582ff.chunk.js.map
│       │   ├── runtime~main.7f5ee7e1.js
│       │   └── runtime~main.7f5ee7e1.js.map
│       └── media
│           └── logo.5d5d9eef.svg

```

After you are done with this, it remains only to add a location /ayaat context your nginx file `/etc/nginx/sites-available/playquran.com`

```
server {
root /var/www/playquran.com/html;
server_name playquran.com www.playquran.com;
location / {

        }
location /ayaat/ {
 try_files $uri /ayaat/index.html;
 }
...
}
```

Note how we specified the root location under the server and used the `try_files` inside the location context.

## Conclusion
In this tutorial, I guided you through three digitalocean tutorial links on how to provision your own server and serve static contents over an nginx server. Also, how to serve a nodejs app listening on a particular port. Also, gave you the solution how to serve an express app running on a port.

Finally, I showed you how to build a react front-end app and serve it through a subdirectory on your main domain.

Would you clap if this was helpful :)