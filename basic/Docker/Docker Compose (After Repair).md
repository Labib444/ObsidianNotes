### Networks

```bash
version: '3'
services:
	web: 
		image: nginx:alpine
		container_name: nginx
	db:
		image: mysql:5.7
		container_name: mysql
		networks:
			- test_net
		environment:
			MYSQL_ROOT_PASSWORD=12345
network:
	test_net: #same as docker network create
	#network name is docker-compose_test_net
```

```bash
version: '3'
services:
	web: 
		image: centos
		container_name: nginx
		tty: true
	db:
		image: centos
		container_name: mysql
		networks:
			- test_net
		environment:
			MYSQL_ROOT_PASSWORD=12345
		tty: true
network:
	test_net: #same as docker network create
	#network name is docker-compose_test_net
```

```bash
docker-compose up -d
docker exec -ti nginx bash -c "ping web" #use the service name not the container name it is the best practice.
```

### Building

```bash
version: '3'
services:
	web:
		image: my_image
		container_name: test_container
		build: . #This means the Dockerfile is in the current location
```

```Dockerfile
FROM centos
RUN mkdir /opt/test
```

```bash
docker-compose build
docker-compose up -d
```

###### Renamed Dockerfile 

```bash
version: '3'
services:
	web:
		image: my_image
		container_name: test_container
		build: 
			context: . #current location
			dockerfile: Dockerfile2 #Dockerfile Name
```

### Overwrite CMD

```bash
version: '3'
services:
	something:
		image: centos
		container_name: centos
		command: python -m SimpleHTTPServer
```

```bash
docker-compose up -d
```

