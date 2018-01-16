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

6. **Deployment** - Never do deployments manually. Use [Codeship](http://codeship.com/)(hosted solution) or [Jenkins](https://jenkins.io/) for a complex pipeline

7. Use ```npm shrinkwrap``` to lock down the versions of a package's dependencies. Run the command before pushing your changes to production. ```nsp audit-shrinkwrap``` command to check for vulnerable modules.

8. **VPNs and Private Networks** - 
> A VPN, or virtual private network, is a way to create secure connections between remote computers and present the connection as if it were a local private network. This provides a way to configure your services as if they were on a private network and connect remote servers over secure connections. - Digital Ocean

Once you are on a private network, your communication is secure and you only have to expose the interfaces that your clients need and no need to open up a port for Redis/Postgres, etc.

9. **Log everything** - [Logstash](https://www.elastic.co/products/logstash)

10. **Monitoring and Alerting** - Tools like Zabbix, New Relic, Monit, PagerDuty, etc.

11. **Caching** - Cache everything, not just API caching, but on database level too. Why? Smaller load on servers -> cost-effective infra. Faster responses to clients -> happy users

12. Makes development faster and increases productivity, thanks to more than 230k NPM modules available for instant use.

13. High scalability - Less investment in infrastructure, since we can handle same amount of load with lesser hardware.

14. Each release is maintained for 30 months.