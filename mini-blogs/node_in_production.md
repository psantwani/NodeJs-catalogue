1. Restart application using forever, [PM2](https://github.com/Unitech/pm2)
2. Use [supervisord](http://supervisord.org/) for non-nodejs processes
- Installation ```easy_install supervisor``` (as supervisor is written in Python)
- config
```
[program:my-api]
command=node /home/myuser/myapi/app.js
autostart=true
autorestart=true
environment=NODE_ENV=production
stderr_logfile=/var/log/myapi.err.log
stdout_logfile=/var/log/myapi.out.log
user=myuser
```
```user=myuser``` - Never ever run your application with superuser rights

3. [httpok](http://superlance.readthedocs.org/en/latest/httpok.html), a **Supervisor event listener** - Monitor events like connection of app with database or any service/resource. If something fails, httpok will restart the process. Place the following inside supervisord.conf
```
[eventlistener:httpok]
command=httpok -p my-api http://localhost:3000/healthcheck
events=TICK_5
```
Expose GET /healthcheck in your app

4. **Reverse proxy** - We cant listen on port 80 because we are not using superuser rights. Reverse proxy can help with the following
- Port forwarding
- Offload some tasks from node.js
 - ngnix can perform SSL encryption
 - compress
 - serve static content

 Sample config file inside ```/etc/nginx/sites-available/my-site```
 ```
 server {
    listen 80;

    server_name my.domain.com;

    location / {
        proxy_pass http://localhost:3000;
    }
}
```

5. **Load balancing** - Use HAProxy or CDN with load balancing functionality. Use [keepalived](http://www.keepalived.org/) to avoid HAProxy becoming a single point of failure