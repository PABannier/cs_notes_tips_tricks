# Nginx

## What is Nginx?

Nginx is a lightweight web server. It is usually used as a reverse proxy between the server and the client to ensure load balancing, SSL encryption, etc.

To install Nginx on an Ubuntu server, run:

```
apt install nginx
```

The configuration directory can be found at: `/etc/nginx/`.

## What are sites-available and sites-enabled?

**sites-available**: which services are available to Internet
**sites-enabled**: which services are currently available

A basic configuration file in sites-available looks like:

```
server {
        server_name hello-world.com;

        access_log /var/log/nginx/st-access.log;
        error_log /var/log/nginx/st-error.log debug;

        location / {
                proxy_pass http://localhost:3000;
                proxy_http_version 1.1;
                proxy_buffering off;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_cache_bypass $http_upgrade;
        }

        location /api {
            proxy_pass http://localhost:5000;
            proxy_http_version 1.1;
        }
}
```

You need to create a symbolic link between sites-available and sites-enabled for your newly created file.

```
ln -l <DESTINATION_PATH_SITES_AVAILABLE> <DESTINATION_PATH_SITES_ENABLED>
```

To make the changes effective, run:

```
service nginx restart
```
