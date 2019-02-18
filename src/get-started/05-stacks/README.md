### 1 add a new service in docker-compose.xml
The only thing new here is the peer service to web, named visualizer. Notice two new things here: a volumes key, giving the visualizer access to the host’s socket file for Docker, and a placement key, ensuring that this service only ever runs on a swarm manager -- never a worker. That’s because this container, built from an open source project created by Docker, displays Docker services running on a swarm in a diagram.

We talk more about placement constraints and volumes in a moment.

### 2 re-deploy docker-compose.yml
```
docker stack deploy -c docker-compose.yml getstartedlab
```

### 3 add redis in docker-compose2.yml 
mkdir /root/data in the manager vm
redis always runs on the manager, so it’s always using the same filesystem.
redis accesses an arbitrary directory (/root/data) in the host’s file system as /data inside the container, which is where Redis stores data.
```
  redis:
    image: redis
    ports:
      - "6379:6379"
    volumes:
      - "/root/data:/data"
    deploy:
      placement:
        constraints: [node.role == manager]
    command: redis-server --appendonly yes
    networks:
      - webnet
```

### 4 redeploy
```
docker stack deploy -c docker-compose2.yml getstartedlab
```

### 5 list docker service
```
docker service ls
```

