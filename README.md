# nginx-trouble-shooting
> - Maintained by: `Heegu Park`

## 502 bad gateway error
- If you see this even after you've completed all configurations in nginx, please check your application folder name(probably under `/home/ubuntu/` and nginx config file `/etc/nginx/sites-available/[your app name]`. It should be identical.
- For example, if the folder name of your application is `www.heegu.net`, the config file of your application(`www.heegu.net`) should look like this(this is a sample),
```
server {
        root /home/ubuntu/www.heegu.net/server/public;

        index index.html index.htm index.nginx-debian.html;

        server_name www.heegu.net;

        location / {
                try_files $uri $uri/ /index.html;
        }

        location /api {
                proxy_pass https://localhost:3001;
        }
}
```

## If your npm modules are not loaded when you try to run your node application, please check your node version or module version. 
- `node --version`
- `npm -v`
- `npm ls` to inspect current package/dependency versions
* Compare the versions of your local modules with ones of your nginx

## When you try to open your `subdomain` such as notes.heegu.net, it keeps redirecting to your `www`.
- Please check your https(or certbot) setting in the conf file

1. `cd /etc/nginx/sites-available`
2. `sudo nano [yourappname]`
3. At the very bottom of the conf file, It should look like this 
```
server {
    if ($host = notes.heegu.net) {
        return 301 https://$host$request_uri;
    } # managed by Certbot

    server_name notes.heegu.net;
    listen 80;
    return 404; # managed by Certbot
}
```
4. `sudo service nginx restart`


# When you're using socket.io and it has 'Error during WebSocket handshake: Unexpected response code: 400'
- Please check your nginx setting and add this to your conf

```
        location /[your endpoint] {
                proxy_pass http://[]your domain]:[your port];
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection "upgrade";
                proxy_set_header Host $host;
        }
```


