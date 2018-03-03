# Importance of monitoring
1. You constantly need to detect bottlenecks and figure out what slows the product down. 
2. Handle and preempt downtimes.
3. Profiling on code level - Time taken to run each function on prod, not just locally.
4. Monitoring network connections - If you're building a microservice architecture, you have to monitor n/w connections and lower delays in the communication between the services.
5. Performance dashboard - Knowing and seeing the most important performance metrics of your application is essential
6. Real-time alerting - Pagerduty, Opsgenie.

# Server Monitoring 
- Disk space
- CPU time
- Memory
- Network

# Application Monitoring
- Database connection
- How much request does it handle? (throughput) 
- Response times of individual instances
- Is it up ? Healthz.

Use [Trace](https://trace.risingstack.com/) for monitoring.