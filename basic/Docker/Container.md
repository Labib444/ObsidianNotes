- Runtime representation of an image.
- They are temporary (changes done to the container will be removed if you destroy the container so we need to build an image.)
- The layer is RW (read write permission)
- We can create as many containers as we want from one image.

```bash
docker container ls #same as docker ps
docker rename old_name new_name (rename container)
docker start container_name (it will run the existing stopped container)
docker end container_name
```
- docker run is used when there is no container it will create it and it will start it.
- docker start is used when we already have a container which is stopped also the container will hold it's resources and start with those resources again.

```
docker rm -f $(docker ps -a -q) (it destroys all of your containers)
```
- docker container has a totally different file system.
- ENV defined a docker file can be accessed in the terminal of the container.

```
docker run -d --name env -e "test=4321" env # here e is environment
docker exec -ti container_hash bash
env | grep test

#NOTE!
if you have already defined a environment variable in your Dockerfile and you are again providing a environment variable with the same name in the terminal then the terminal's value with overwrite the value of the Dockerfile. 
```

### MySQL

```
docker run -d --name test mysql:5.7
docker ps (container exited need to pass some environment variables)
docker logs test

docker run -d --name mysql -e MYSQL_ROOT_PASSWORD=12345678 mysql:5.7
docker ps
docker inspect container_hash
```
![[Pasted image 20230225061625.png]]
```
docker ps
sudo yum -y install mysql
ping 172.17.0.2
mysql -u root -h 172.17.0.2 -p # (-u: user, -h: host, -p: password)
#enter password
Bye

docker run -d -p 3306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=1234 -e MYSQL_DATABASE=docker_db -e MYSQL_USER=docker_user -e MYSQL_PASSWORD=1234 mysql:5.7

mysql -u root -h 127.0.0.1 -p12345678
mysql -u docker_user -h 172.31.87.182 -p1234
```
- use the EXPOSE command to expose ports just like the -p in docker run.
- EXPOSE 80 equals to -p 80:80

```bash
docker run -d --name jenkins -p 9090:8080 jenkins/jenkins
docker ps
docker exec -ti jenkins bash
cat /var/jenkins_home/secrets/initialAdminPassword

```

### Limit container resources

```bash
free -h # how much ram available
grep "model name" /proc/cpuinfo # shows cpu info
grep "model name" /proc/cpuinfo | wc -l # shows cpu counts
docker run -d --name ngnix nginx:alpine
docker stats container_name

docker run -d -p 3306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=1234 -e MYSQL_DATABASE=docker_db -e MYSQL_USER=docker_user -e MYSQL_PASSWORD=1234 mysql:5.7
docker stats mysql

docker run --help
docker run --help | grep memory

docker run -d --name nginx2 --memory "200mb" nginx:alpine # limits to 200mb ram
docker stats container_hash container_hash_2

docker run --help | grep cpu
# --cpuset-cpus
# can assign number of cpus
docker run -d --name nginx2 --memory "200mb" --cpuset-cpus 0 nginx:alpine
docker run -d --name nginx2 --memory "200mb" --cpuset-cpus 0-3 nginx:alpine
docker run -d --name nginx2 --memory "200mb" --cpuset-cpus 0,3 nginx:alpine
# cpu count start from zero 
```

### Copy Files

```bash
docker run --name apache -p 8080:80 -d httpd
docker ps
docker exec -ti container_hash bash
pwd
ls
cd htdocs/
cat index.html
cat /usr/local/apache/htdocs/index.html
exit
```

###### Copying file from host to docker

```bash
cd docker-images/
vi test.html
COPIED FILE FROM OUTSIDE
cat test.html
docker cp test.html apache:/usr/local/apache/htdocs/index.html
```

###### Copying file from docker to host

```bash
docker cp apache:/usr/local/apache/htdocs/index.html .
```

### Create an image from a container

```bash
docker run -d -p 8080:80 --name test httpd
docker exec -ti test bash
vi htdocs/index.html
echo HELLO > htdocs/index.html
touch /opt/test1
ls /opt/
# test1
exit

docker rm -f test
docker run -d -p 8080:80 --name test httpd
ls /opt/
# empty
exit
```

```bash
docker run -dti --name centos centos
docker ps
docker exec -ti container_hash bash
cd /opt/
ll
echo FILE1 > file1.txt
cat file1.txt

mkdir test_dir
echo FILE2 > file2.txt
ll
cd test_dir/
ls
exit

docker commit container_hash final_test_centos_image:v1
docker images | grep final_test_centos_image
docker rm -f prev_container_hash
docker run -dti --name test final_test_centos_image
docker ps
docker exec -ti final_test_centos_image
cd /opt/
ls
# your file is still there.
```


### Writing CMD instruction in the terminal command

```bash
docker run -dti --name centos centos
docker ps
docker exec -ti container_hash bash
python
python -m SimpleHTTPServer 8080


docker run -dti --name centos2 centos python -m SimpleHTTPServer 8080
docker ps
docker ps --no-trunc
docker rm -f centos2

docker run -dti --name centos2 -p 9090:8080 centos python -m SimpleHTTPServer 8080
docker ps

```

### How to destroy the container automically after exiting the container

```bash
docker run --rm -ti centos bash
ls /opt/
ls /tmp/
exit

docker ps
# donot have the container
```
- might help in debugging

### Where are the containers stored and change directory of docker

```bash
docker info | grep -i root
sudo du -sh /var/lib/docker
cd /var/lib/docker


# copying docker folder to somewhere else
docker ps
systemctl stop docker
vi /lib/systemd/system/docker.service
# add --data-root /mnt/docker
```

![[Pasted image 20230227052412.png]]

```bash
systemctl daemon.reload
systectm restart docker
docker info | grep -i root
docker ps
docker images # no images where are they?
du -sh /mnt/docker/
du -sh /var/lib/docker
systemctl stop docker
cd /var/lib/docker
rm -rf /mnt/docker/
ls /mnt/
mv docker /mnt/
ls
pwd
ls /mnt/
ls /mnt/docker/
system restart docker
docker images # all now is showed
docker info | grep -i root
```