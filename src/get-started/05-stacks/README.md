### 1 add a new service in docker-compose.xml
The only thing new here is the peer service to web, named visualizer. Notice two new things here: a volumes key, giving the visualizer access to the host’s socket file for Docker, and a placement key, ensuring that this service only ever runs on a swarm manager -- never a worker. That’s because this container, built from an open source project created by Docker, displays Docker services running on a swarm in a diagram.

We talk more about placement constraints and volumes in a moment.

### 2 re-deploy docker-compose.xml
```
docker stack deploy -c docker-compose.yml getstartedlab
```