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
