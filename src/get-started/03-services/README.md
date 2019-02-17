### 1. swarm init
```
docker swarm init
```

### 2. docker stack deploy
```
docker stack deploy -c docker-compose.yml getstartedlab
```

### 3. docker service
```
docker service ls

```
### 4. check service
```
docker service ps getstartedlab_web
```

### 5. docker container
```
docker container ls -q
```

### 6. curl
```
curl -4 http://localhost:4000
```

### 7. modify replicas in docker-compose.yml

### 8. hot deploy
```
docker stack deploy -c docker-compose.yml getstartedlab
```
### 9. list deployed instances reconfigured
```
docker container ls -q
```

### 10. Take down the app and the swarm
```
docker stack rm getstartedlab
docker swarm leave --force
```
### 0. cheetsheet
```
docker stack ls                                            # List stacks or apps
docker stack deploy -c <composefile> <appname>  # Run the specified Compose file
docker service ls                 # List running services associated with an app
docker service ps <service>                  # List tasks associated with an app
docker inspect <task or container>                   # Inspect task or container
docker container ls -q                                      # List container IDs
docker stack rm <appname>                             # Tear down an application
docker swarm leave --force      # Take down a single node swarm from the manager
```