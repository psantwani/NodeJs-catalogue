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