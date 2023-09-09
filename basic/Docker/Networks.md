- Bridge Network
- Host Network
- None Network

```bash
ip a
ip a | grep docker
```
- Default network is bridge network
```bash
docker network ls
docker network ls | grep bridge
docker network inspect

```
![[Pasted image 20230228060011.png]]
```bash 
docker inspect container_hash
```
![[Pasted image 20230228060104.png]]
- the container was linked to bridge by default. Because you can see that Network object has a bridge object. here Ip is 172.17.0.2 because the bridge has a subnet of 172.17.0.0/16. code down below.
```bash
ip a
```
![[Pasted image 20230302034952.png]]
- docker0 handles all the networks between containers
![[Pasted image 20230302035019.png]]
- inet 172.17.0.1/16 is the gateway address

```bash
docker network inspect bridge
```
![[Pasted image 20230302035231.png]]
![[Pasted image 20230302035649.png]]

![[Recording 20230302035825.webm]]

### Default Network: Bridge

```bash 
docker network ls
```
![[Pasted image 20230302035958.png]]
- Driver is important, "Name" doesn't matter.

- pinging containers.
![[Pasted image 20230302040535.png]]
![[Pasted image 20230302040613.png]]

### Creating Docker Network

```bash
docker network create
docker network create --help
```

```bash
docker network inspect bridge #check subnet ip need to avoid overlapping
docker network create --help
docker network create -d bridge --subnet 172.18.0.0/16 --gateway 172.18.0.1 new_network_name

docker network ls
docker network inspect new_network_name
```

###### Removing network

```bash
docker network rm new_network_name
docker network ls
```

### Embedded DNS

```bash
docker run -dti --network new_network_name --name container1 centos
docker run -dti --network new_network_name --name container2 centos

docker exec container1 bash -c "ping container2"
docker exec container1 bash -c "ping container1"
```

- We cannot ping with container name in default networks but can in custom networks.

### Connect and Disconnect Networks

```bash
docker run -dti --name net1_cont --network net1 centos
docker run -dti --name net2_cont --network net2 centos

docker exec net1_cont bash -c "ping net2_cont" #Error because they are in different networks.

docker network connect net1 net2_cont #We are connecting net2_cont container to network net1
docker exec net1_cont bash -c "ping net2_cont" #Success

docker network disconnect net1 net2_cont
docker exec net1_cont bash -c "ping net2_cont" #Error
```

### Static IP in a Container

- can only assign static ip to custom network cannot do it in default networks.
![[Pasted image 20230302043057.png]]

### Host Network

- Your container will inherit everything from the docker host. Same IP.

```bash
docker run --rm -ti --network host centos bash
```

### None Network

![[Pasted image 20230302043644.png]]

```bash
docker run -dti --name none --network none centos
docker ps -l
```

![[Pasted image 20230302043908.png]]
![[Pasted image 20230302043822.png]]
