
### Context

Basically means current directory in the Dockerfile.
- du -shc * (current folder of Dockerfile)
- cd ..
- docker build -t image_name -f Dockerfile3
	--build-arg user=ricardo --build-arg httpd_package=httpd
	.
	(Error)
- docker build -t image_name -f docker-images/Dockerfile3
	--build-arg user=ricardo --build-arg httpd_package=httpd
	.
	(Error)
- docker build -t image_name -f docker-images/Dockerfile3
	--build-arg user=ricardo --build-arg httpd_package=httpd
	docker-images/ 
	(Success)

### Ignore files

docker build -t image_name -f docker-images/Dockerfile3
	--build-arg user=ricardo --build-arg httpd_package=httpd
	. (Error but context size is 79MB)

docker build -t image_name -f docker-images/Dockerfile3
	--build-arg user=ricardo --build-arg httpd_package=httpd
	docker-images/ (success but context size is 5MB)

Because: in the first example in that same folder there are older files and folders and docker takes them into the context too. Those files and folders have much higher size.

```Bash
#create this file in the root of your context.
vi .dockerignore 
```
![[Pasted image 20230211162521.png]]

```Bash
vi .dockerignore

#inside file paste the folder names
linuxfacilito.online
templates

docker build -t image_name -f docker-images/Dockerfile3
	--build-arg user=ricardo --build-arg httpd_package=httpd
	. (Error but context size is 7MB)
```

### Docker Best Practices

- One service per container
- Build context should be small
- Avoid unncessary packages
- Less layers

```bash
vi Dockerfile4
```

```Dockerfile
FROM nginx:alpine 
RUN echo "1" >> /usr/share/nginx/html/test.html
RUN echo "2" >> /usr/share/nginx/html/test.html
RUN echo "3" >> /usr/share/nginx/html/test.html
#Too many layers
```

```bash
docker build -t test -f Dockerfile4 .
docker run -d -p 9090:80 test
```

```Dockerfile
FROM nginx:alpine 
RUN \ 
	echo "1" >> /usr/share/nginx/html/test.html && \
	echo "2" >> /usr/share/nginx/html/test.html && \
	echo "3" >> /usr/share/nginx/html/test.html
#One layer
```

```Dockerfile
FROM nginx:alpine 
ENV webfile /usr/share/nginx/html/test.html
RUN \ 
	echo "1" >> $webfile && \
	echo "2" >> $webfile && \
	echo "3" >> $webfile
#One layer much better.
```
### hosting php

```bash
mkdir ssl
cd ssl (this folder contains another folder which has index.html and other web          files. folder name is html-ssl)

vi Dockerfile
```

```Dockerfile
FROM centos
RUN yum -y install httpd php php-mysql
COPY html-ssl /var/www/html
RUN echo "<?php phpinfo(); ?>" >/var/www/html/test.php
CMD apachectl -DFOREGROUND
```

```bash
docker build -t apache_ssl:v1 .
docker run -d -p 9090:80 --name apache_ssl apache_ssl:v1
docker ps
docker rm -f gifted_kalam (this image was using the 9090 port)
```
### SSL

```bash
openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 -keyout docker.key -out docker.crt

#enter all just until Common Name(eg, your name...)
#just the enter the docker image ip
#enter all

ll
```
![[Pasted image 20230211170858.png]]

```Dockerfile
FROM centos
RUN yum -y install httpd php php-mysql mod_ssl openssl
COPY html-ssl /var/www/html
RUN echo "<?php phpinfo(); ?>" >/var/www/html/test.php
COPY docker.crt /docker.crt
COPY docker.key /docker.key
COPY ssl.conf /etc/httpd/conf.d/default.conf
CMD apachectl -DFOREGROUND
```

```bash
vi ssl.conf

<VirtualHost *:443>
	SSLEngine on
	SSLCertificateFile /docker.crt
	SSLCertificateKeyFile /docker.key
	ServerAdmin example@it.com
	DocumentRoot "/var/www/html"
	ServerName 35.171.121.237
	ErrorLog /var/www/html/error.log
</VirtualHost>
```

```bash
docker build -t apache_ssl:v3 .
docker ps -a -f=name=apache (filtering containers)
docker rm -f CONTAINER_ID

docker build -t apache_ssl:v3 .
docker run -d -p 443:443 --name apache_ssl3 apache_ssl:v3
```

### Dangling Image

![[Pasted image 20230211174920.png]]
- images are readonly we cannot do or add new modifications.
- If modification is needed then you must create new image.
- If by any chance you build with same image name then dangling image will be created.
![[Pasted image 20230211175428.png]]
- Use tags to avoid dangling images.
```bash
#Removing none images
docker images -f dangling=true
docker images -f dangling=true -q (prints with ids)
docker rmi $(docker images -f dangling=true -q) (removes all dangling images)
```

### Making image with latest php

mkdir nginx
cd nginx/
ll
vi Dockerfile-nginx

```Dockerfile
FROM centos
COPY conf/nginx.repo /etc/yum.repos.d/nginx.repo
RUN yum install -y nginx && \ 
	yum install -y https://centos7.iuscommunity.org/ius-release.rpm && \
	yum install -y \ 
		php71u-fpm \
		php71u-common \
		php71-cli --enablerepo=ius && yum clean all
```
- yum clean all will clean all temporary files
- rule is docker image should as small as possibe.

vi nginx.repo
```bash
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/mainline/centos/7/$basearch/
gpgcheck=0
enabled=1
```
ll
mkdir repo
mv nginx.repo repo/
ls repo/
mv repo/ conf
ls conf/

```bash
docker build -t nginx:v1 -f Dockerfile-nginx .
```

- copy from [Example vhost config for Nginx (PHP) · GitHub](https://gist.github.com/lukearmstrong/7155390)
```
vi conf\ (paste the code from the github)
```

```
server {
	listen 80;
	#server_name .example.co.uk.dev;
	server_name 35.171.121.237:90; (change it to your desire)

	access_log /nginx_php/access.log; (change it)
	error_log  /nginx_php/error.log error; (change it)

	root /nginx_php; (change it to your desire)
	index index.php index.html;

	location / {
		try_files $uri $uri/ /index.php?$query_string;
	}

	location ~ \.php {
		fastcgi_index index.php; (change it)
		fastcgi_pass 127.0.0.1:9000; (change it)

		include fastcgi_params;
		fastcgi_split_path_info ^(.+\.php)(/.+)$;
		fastcgi_param PATH_INFO $fastcgi_path_info;
		fastcgi_param PATH_TRANSLATED $document_root$fastcgi_path_info;
		fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
	}

	# Prevents caching of css/less/js/images, only use this in development
	location ~* \.(css|less|js|jpg|png|gif)$ {
		add_header Cache-Control "no-cache, no-store, must-revalidate"; 
		add_header Pragma "no-cache";
		expires 0;
	}
}
```

- problem: If we want to run phpa and nginx we have manually run it from the terminal. It is not possible from the docker if your thinking to run it in forground mode. Therefore we have go inside the image and run the terminal and run some commands. To automate it we have to write a script.

```
vi Dockerfile-Nginx

FROM centos
COPY conf/nginx.repo /etc/yum.repos.d/nginx.repo
RUN yum install -y nginx && \ 
	yum install -y https://centos7.iuscommunity.org/ius-release.rpm && \
	yum install -y \ 
		php71u-fpm \
		php71u-common \
		php71-cli --enablerepo=ius && yum clean all

RUN mkdir /nginx_php
COPY conf/nginx.conf /etc/nginx/conf.d/default.conf

#save it

docker build -t nginx:v1 -f Dockerfile-Nginx
docker ps
docker rm -f apache_ssl

docker build -t nginx:v1 -f Dockerfile-Nginx .
docker run -d --name test -p 9090:80
docker rm -f test
docker run -dti --name test -p 9090:80 nginx:v1

docker exec -ti test bash
ps aux
/usr/sbin/php-fpm
ps aux
nginx -g 'daemon off;'


docker rm -f test
docker build -t nginx:v1 -f Dockerfile-Nginx .
docker run -dti --name test -p 9090:80 nginx:v1
docker exec -ti test bash
ps aux
/usr/sbin/php-fpm (running a command)
nginx -g 'daemon off;'
```

```
vi conf/start.sh
#!/bin/bash

#Starting php
echo "*** starting php"
/usr/sbin/php-fpm

#Start nginx
echo "*** starting nginx"
nginx -g 'daemon off;'

#save it

vi Dockerfile-Nginx
COPY conf/nginx.conf /etc/nginx/conf.d/default.conf
COPY conf/start.sh /start.sh
RUN chmod +x /start.sh
CMD /start.sh
docker build -t nginx:v1 -f Dockerfile-Nginx .
docker rm -f test
docker ps
docker build -t nginx:v1 -f Dockerfile-Nginx .
docker run -d --name test -p 9090:80 nginx:v1
docker ps
```

```bash
vi Dockerfile-Nginx
...
...
...
COPY conf/nginx.conf /etc/nginx/conf.d/default.conf
COPY fruit /nginx_php # (ui pages)
COPY test.php /nginx_php
COPY conf/start.sh /start.sh
RUN chmod +x /start.sh
CMD /start.sh
```

### Multistage

```bash
mkdir multi-stage
ll
cd multi-stage/
vi Dockerfile

FROM centos
#save it

docker images | grep centos (watch the size 202 MB)
docker build -t test-multi .
docker images | grep multi

fallocate -l 10M file1 (creating a file size of 10MB)
du -sh file1
rm -rf file1
```

```
FROM centos
RUN fallocate -l 10M /opt/1
RUN fallocate -l 10M /opt/2
RUN fallocate -l 5M /opt/jar
#save it

docker images | grep multi
docker pull alpine
docker images | grep alpine
```

```
FROM centos as build
RUN fallocate -l 10M /opt/1
RUN fallocate -l 10M /opt/2
RUN fallocate -l 5M /opt/jar

FROM alpine 
COPY --from=build /opt/jar /jarfile.jar
#save it

docker build -t test-multi .
```

```bash

FROM maven:3.6-alpine


FROM openjdk:8-alpine

```
- git clone https://github.com/jenkins-docs/simple-java-maven-app.git
```
mv simple-java-maven-app/ app-maven
ll
vi Dockerfile
```
```
FROM maven:3.6-alpine
COPY app-maven /app
WORKDIR /app
RUN mvn package

#FROM openjdk:8-alpine
#save it
#build docker

vi Dockerfile
```
```bash
FROM maven:3.6-alpine as builder
COPY app-maven /app
WORKDIR /app
RUN mvn package

FROM openjdk:8-alpine
COPY --from=builder /app/target/my-app-1.0-SNAPSHOT.jar /app.jar
CMD java -jar /app.jar
```

![[Recording 20230227080828.webm]]

###