> Configure the Docker hosts
```
Manager Node – 172.18.111.101 (hostname - dockermanager)
Worker Node1 – 172.18.111.102 (hostname – dockerworker1)
Worker Node2 – 172.18.111.103 (hostname - dockerworker2
```
> Install and Run Docker Service
```
# apt-get update
# apt-get install apt-transport-https ca-certificates curl software-properties-common -y
# curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
# add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
# apt-get update
# apt-get install docker-ce
# systemctl start docker
# systemctl enable docker
# usermod -aG docker <username>
# usermod -aG docker manager
# usermod -aG docker worker1
# usermod -aG docker worker2
```
> Configure the Manager Node for Swarm Cluster Initialization
```
# docker swarm init --advertise-addr 172.18.111.101
```
>Configure Worker Nodes to join the Swarm Cluster
```
$ docker swarm join --token SWMTKN-1-4htf3vnzmbhc88vxjyguipo91ihmutrxi2p1si2de4whaqylr6-3oed1hnttwkalur1ey7zkdp9l 172.18.111.101:2377
```
> Verify the Swarm Cluster
```
$ docker node ls
```
>Deploy new Service on Swarm Cluster
```
$ docker service create --name my-web1 --publish 8081:80 --replicas 2 nginx
```
>To check the newly created service
```
$ docker service ls
$ docker service ps my-web1
```
> scaling replicas
```
docker service scale my-web1=3
```
