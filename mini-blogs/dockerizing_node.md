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