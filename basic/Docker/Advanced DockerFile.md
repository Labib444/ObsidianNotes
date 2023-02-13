
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