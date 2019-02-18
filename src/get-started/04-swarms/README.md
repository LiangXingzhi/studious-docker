### 1.1 Create 2 CentOS7 VM using virtualbox
CentOS701 192.168.56.101
CentOS702 192.168.56.102
### 1.2 install docker on the VMs
### 1.3 stop firewall
```
systemctl stop firewalld
chkconfig firewalld off
``` 
### 1.4 init swarm on docker manager
```
docker swarm init --advertise-addr 192.168.56.101
```
### 1.5 join swarm on docker worker
token is specified in the results of swarm init command
```
docker swarm join --token 
```
### 1.6 deploy or redeploy
getstartedlab is the stack name
```
docker stack deploy -c docker-compose.yml getstartedlab
```
```
docker stack ps getstartedlab
```
check the service status if the service is fully started
```
docker service ls
```

If need redeploy just modify the docker-compose.yaml and rerun the above commands.

### 1.7 Cleanup and reboot
remove stack
```
docker stack rm getstartedlab
```
leave swarm on worker node
```
docker swarm leave
```
leave swarm on manager node
```
docker swarm leave --force
```
### 2.1 Create a Cluster Windows10
First, quickly create a virtual switch for your virtual machines (VMs) to share, so they can connect to each other.

1.1 Launch Hyper-V Manager
1.2 Click Virtual Switch Manager in the right-hand menu
1.3 Click Create Virtual Switch of type External
1.4 Give it the name myswitch, and check the box to share your host machine’s active network adapter

Now, create a couple of VMs using our node management tool, docker-machine:
Hyper-v commands have to be run as an Administrator
```
docker-machine create -d hyperv --hyperv-virtual-switch "myswitch" myvm1
docker-machine create -d hyperv --hyperv-virtual-switch "myswitch" myvm2
```
```
docker-machine ls
```
```
docker-machine env myvm1

myvm1 "tcp://100.98.100.230:2376"
myvm2 "tcp://100.98.102.254:2376"
```

### 2.2 initialize swarm and add nodes
myvm1 init as docker manager
```
docker-machine ssh myvm1 "docker swarm init --advertise-addr 100.98.100.230"

Swarm initialized: current node (z6kvxvq1kpc4lldibnzau4zq5) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-5bs7ynupjsmhqr2vemu260q6c04acatni6xl303v8ft33qvmic-391sc8nh9h4f5k8p4evfpm71n 100.98.100.230:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.

```
myvm2 join myvm1 as a worker
```
docker-machine ssh myvm2 "docker swarm join \
--token SWMTKN-1-5bs7ynupjsmhqr2vemu260q6c04acatni6xl303v8ft33qvmic-391sc8nh9h4f5k8p4evfpm71n \
100.98.100.230:2377"

This node joined a swarm as a worker.
```
```
docker-machine ssh myvm1 "docker node ls"
```

### 2.3 deploy app to swarm cluster
```
docker-machine env myvm1

$Env:DOCKER_TLS_VERIFY = "1"
$Env:DOCKER_HOST = "tcp://100.98.100.230:2376"
$Env:DOCKER_CERT_PATH = "C:\Users\ezliagu\.docker\machine\machines\myvm1"
$Env:DOCKER_MACHINE_NAME = "myvm1"
$Env:COMPOSE_CONVERT_WINDOWS_PATHS = "true"
# Run this command to configure your shell:
# & "C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe" env myvm1 | Invoke-Expression

```
Run the given command to configure your shell to talk to myvm1
```
& "C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe" env myvm1 | Invoke-Expression
```
Run docker-machine ls to verify that myvm1 is the active machine as indicated by the asterisk next to it.

```
docker-machine ls

NAME    ACTIVE   DRIVER   STATE     URL                         SWARM   DOCKER     ERRORS
myvm1   *        hyperv   Running   tcp://100.98.100.230:2376           v18.09.2
myvm2   -        hyperv   Running   tcp://100.98.102.254:2376           v18.09.2
```

deploy to swarm manager myvm1
```
docker login
docker stack deploy -c docker-compose.yml getstartedlab
```

