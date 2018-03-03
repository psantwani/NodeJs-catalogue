# Immutable Infrastructures

It consists of data and everything else. The everything else part is replaced on each deploy. 
### Machine based approach
On each deploy you setup entirely new EC2 machines and deploy your app on them. Modify load balancer to point to new machines and delete older ones.
### Container based approach
Multiple containers running on the same machine (Docker). VMVare/VBox can be used too, but Docker boots faster.

Advantages of Immutable Infra -
- Going back to older versions is easy
- Testing new infra in isolation

# Docker and Codeship
- Push to master branch, Github will trigger a build on [Codeship](http://codeship.com/)
- If everything is OK, Codeship triggers a build on [Docker Hub](https://hub.docker.com/)
- After new Docker image is ready (pushed), Docker triggers a webhook
- Ansible pull the latest image to the application servers (Docker Deployer)

# Containers and Docker

**Hypervisor virtualization** - They (VMVare, Oracle VirtualBox, etc) emulates the hardware. Both host and guest OS run their own kernel and the guest communicates with actual hardware via abstracted layer of hypervisor. This is slow because of hardware emulation.
**Operating system virtualization** - Allows running multiple isolated user space instances on the same kernel. They provide a lightweight virtual environment that groups and isolates a set of processes and resources such as memory, CPU, disk. Its faster than the hypervisor approach, but has its shortcomings too. Type of containers should work with the kernel of the host. Cant install windows container on linux host (or viceversa). Less security since using the process in containers have managed into the kernel space of the host.
**Applications** - OS containers and application containers.
**OS Containers** - Useful when you want to run a fleet of identical or different flavors of distros. Container technologies like LXC, OpenVZ, Linux VServer, BSD Jails and Solaris zones are all suitable for creating OS containers.
**Application Containers** - Designed to run multiple processes and services. Docker and Rocket are examples.

# Minimal Docker containers for Node.js
Alpine based node containers - Alpine is a light-weight linux distribution. [This](https://hub.docker.com/r/risingstack/alpine/tags/) minimal docker image of alpine and nodejs is only 90MB.

# Deploying Docker images
Once you build your docker image, you can run it everywhere - most environments will simply ```docker pull``` your image and run it.
Providers - AWS BeanStalk, Heroku Docker Support, Kubernetes.

# Moving from PaaS (like Heroku) to Kubernetes
- Advantages of PaaS: fast deploys, simple scaling, zero downtime deployment, rollback capabilities, environment variable management, zero DevOps
- Disadvantages of PaaS: big network latencies between services, lack of VPC, response time peaks due to multilatency, larger bills
- Kubernetes: Its configuration-based, container-based and microservices friendly. Its an open-source system for automating deployments, scaling and management of containerized deployments.
- Terms
    - ```pod``` : running containerized app. 
    - ```deployment``` : config of your app that describes what state do you need (CPU, memory, env.vars, docker image version, number of instances, etc.)
    - ```secret``` : credentials
    - ```service```: exposing pods as labels to outside world. 
- Getting started with Kubernetes - Create a container engine in Google Cloud, which is a hosted kubernetes. Kubernetes can run anywhere - AWS, DigitalOcean, Azure, etc.
- Procfile : One Docker image for every application.
- Environments : Use namespaces. Each namespace is totally independent from each other and doesnt share any resources like secrets, config, etc.
```
$ kubectl create namespace production
$ kubectl create namespace staging
```
- Application version: Change the image tag on your application on every deploy (configurable in application's deployment config). You can rollback to this tag version anytime.
- Service Discovery: The creatd services expose their hostname and a port as an env variable for each pod.
```
const fooServiceUrl = `http://${process.env.FOO_SERVICE_HOST}:${process.env.FOO_SERVICE_PORT}`
```

# Production ready with Kubernetes
- Zero downtime deployment and failover: Use deployment strategies to always keep some pods running and deploy your changes in smaller steps - instead of stopping and starting them all at the same time.
- Graceful stop: 
## Start Server
```
const server = MyServer()
Promise.all([
   db1.connect()
   db2.connect()
])
  .then() => server.listen(3000))
```
## Graceful server stop
```
process.on('SIGTERM', () => {
  server.close()
    .then() => Promise.all([
      db1.disconnect()
      db2.disconnect()
    ])
   .then(() => process.exit(0))
   .catch((err) => process.exit(-1))
})
```
- Liveness probe (health check): Define health check for your app, so that kubernetes could restart app if needed.
```
app.get('/healthz', function (req, res, next) {
  // check my health
  // -> return next(new Error('DB is unreachable'))
  res.sendStatus(200)
})
```
```
livenessProbe:
    httpGet:
      # Path to probe; should be cheap, but representative of typical behavior
      path: /healthz
      port: 3000
    initialDelaySeconds: 30
    timeoutSeconds: 1
```
- Readiness probe: This check on the webserver tells Kubernetes services (eg load balancer) that traffic can be redirected to the specific pod.

- Github repo following the above practices - https://github.com/RisingStack/kubernetes-nodejs-example