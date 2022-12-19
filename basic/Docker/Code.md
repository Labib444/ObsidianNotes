### Get  Image

1. Go to Docker Hub
2. docker pull mysql
3. docker pull mysql:5.7


### Create container

1. docker run -d nginx:alpine
2. docker ps -l
3. docker rmi nginx:alpine
4. docker run -d nginx:alpine( because we deleted the image docker will download or pull it from the hub.)
5. docker ps
6. docker rm container_name
7. docker rm -f container_name
8. docker ps
9. docker run -d -p 9090:80 nginx:alpine (outputs docker hash id)
10. docker ps

### Docker File

If there is no image in the hub then we have to create our own
image.
1. mkdir docker-images
2. ll
3. vi Dockerfile
```Dockerfile
FROM centos
RUN yum -y install httpd
```
4. cat Dockerfile
5. docker build --tag centos_apache:v1 . ( --tag refers to image name )
6. docker build --tag centos_apache:v1 . (won't download again)
7. vi Dockerfile
```Dockerfile
FROM centos
RUN yum -y install httpd curl
```
8. docker build --tag centos_apache:v1 .
9. docker images
10. docker run -d centos_apache:v1 --name centos-test
11. docker ps
12. docker ps -a
13. docker rm centos-test
14. vi Dockerfile
```Dockerfile
FROM centos
RUN yum -y install httpd curl
//cannot use sudo or systemd need to use something else to run the httpd
CMD apachectl -DFOREGROUND
```
15. docker build --tag centos_apache:v2 .
16. docker images
17. docker run -d --name centos-nocmd centos_apache:v1
18. docker run -d --name centos-yescmd centos_apache:v2
19. docker ps --no-trunc
20. docker run -d -p 9090:80 --name centos-yescmd centos_apache:v2
21. docker rm -f centos-yescmd centos-nocmd