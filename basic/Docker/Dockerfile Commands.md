### Commands

FROM commands start from the given
OS, image, docker image. If given image then it inherits the image's settings such as selected OS.

###### FROM
- vi Dockerfile2
- OS, Another Image, Docker Image
- cat Dockerfile2
```Dockerfile
FROM nginx:alpine
```
- docker build  -t nginx_custom:v1 -f Dockerfile2

###### RUN

```Dockerfile
FROM nginx:alpine
RUN echo "Overwrite home page" > /usr/share/nginx/html/index.html
```
- docker build -t nginx_custom:v2 -f Dockerfile2 .

### COPY

By default in centos we need to put html code in /var/www/html, therefore we need to tell docker to put our files in /var/www/html. We have to keep Dockerfile and html folder in same location to work. 

- Folder
	- Dockerfile
	- Website (containing index.html)
	
```Dockerfile
FROM centos
RUN yum -y install httpd
COPY html-code /var/www/html
CMD apachectl -DFOREGROUND
```
- docker build -t apache_with_code:v1 -f Dockerfile2
- docker ps
- docker rm -f nginx
- docker run -d --name apache -p 9090:80 apache_with_code:v1
- docker ps

### ADD

Downloading something from the internet and copying  to a specific folder.

- cp Dockerfile Dockerfile3
- vi Dockerfile3
```Dockerfile
FROM centos
RUN yum -y install httpd
ADD https://github.com/...../index.zip /var/www/html/code.zip
CMD apachectl -DFOREGROUND
```
- docker build -t apache_with_code: v2 -f Dockerfile3
- vi Dockerfile3
```Dockerfile
FROM centos
RUN yum -y install httpd unzip
ADD https://github.com/...../index.zip /var/www/html/code.zip
RUN cd /var/www/html/ && unzip code.zip && mv /var/www/html/unziped_folder_name/* /var/www/html (moving contents of the unziped folder to the apache root directory.)
CMD apachectl -DFOREGROUND
```
- docker build -t apache_with_code:v3 -f Dockerfile3
- docker rm -f apache (removing previous container)
- docker run -d --name apache -p 9090:80 apache_with_code:v3

### ENV

ENV is used for creating **environment variable**.

```Dockerfile
FROM centos
RUN yum -y install httpd unzip
ENV HTML beginner-html-site-styled
ADD https://github.com/$HTML/index.zip /var/www/html/code.zip
RUN cd /var/www/html/ && unzip code.zip && mv /var/www/html/$HTML/* /var/www/html (moving contents of the unziped folder to the apache root directory.) && echo $HTML > /var/www/html/env.html
CMD apachectl -DFOREGROUND
```

### WORKDIR

Changing the working directory.

```Dockerfile
FROM centos
RUN yum -y install httpd unzip
ENV HTML beginner-html-site-styled
WORKDIR /var/www/html/ (changing directory to the specified in centos)
ADD https://github.com/$HTML/index.zip ./code.zip
RUN unzip code.zip && mv $HTML/* . (moving contents of the unziped folder to the apache root directory.) && echo $HTML > ./env.html (. means current directory)
CMD apachectl -DFOREGROUND
```

### LABEL

It is metadata. Kind of comments in programming languages.

```Dockerfile
FROM centos
RUN yum -y install httpd unzip
LABEL maintainer=Ricardo
LABEL vendor=companyB
ENV HTML beginner-html-site-styled
WORKDIR /var/www/html/ (changing directory to the specified in centos)
ADD https://github.com/$HTML/index.zip ./code.zip
RUN unzip code.zip && mv $HTML/* . (moving contents of the unziped folder to the apache root directory.) && echo $HTML > ./env.html (. means current directory)
CMD apachectl -DFOREGROUND
```

### Accesing the container from terminal

- docker exec -ti container_name bash (accessing inside the container)
	- -ti interactive terminal
	- exec to access the container
	- bash select terminal language
- pwd
- ll
- exit

### USER

Deleting a file by accessing a container from terminal won't delete it terminal if we start the container again or build it again.

```Dockerfile
FROM centos
RUN yum -y install httpd unzip
LABEL maintainer=Ricardo
LABEL vendor=companyB
ENV HTML beginner-html-site-styled
WORKDIR /var/www/html/ (changing directory to the specified in centos)
ADD https://github.com/$HTML/index.zip ./code.zip
RUN unzip code.zip && mv $HTML/* . (moving contents of the unziped folder to the apache root directory.) && echo $HTML > ./env.html (. means current directory)
RUN useradd application
USER application
RUN rm -rf code.zip $HTML-gh-pages/
CMD apachectl -DFOREGROUND
```
- docker exec -ti apache10 bash
- useradd application
- su application
- whoami
- rm -rf code.zip (permission denied)
- exit
- cd ..
- chown application:application html -R

```Dockerfile
RUN useradd application && chown application:application /var/www/html -R
```

- docker logs container_name (run to learn about error.)

Changing user to root.

```Dockerfile
USER application
RUN rm -rf code.zip $HTML-gh-pages/
USER root
CMD apachectl -DFOREGROUND
```

### ARG

ARG can be used to pass arguments in the docfile from the cli command.

```Dockerfile
FROM centos
ARG user=application //setting default value
ARG httpd_package

RUN yum -y install $httpd_package unzip
LABEL maintainer=Ricardo
LABEL vendor=companyB
ENV HTML beginner-html-site-styled
WORKDIR /var/www/html/ (changing directory to the specified in centos)
ADD https://github.com/$HTML/index.zip ./code.zip
RUN unzip code.zip && mv $HTML/* . (moving contents of the unziped folder to the apache root directory.) && echo $HTML > ./env.html (. means current directory)
RUN useradd $user && chown $user:$user /var/www/html -R
USER $user
RUN rm -rf code.zip $HTML-gh-pages/
USER root
CMD apachectl -DFOREGROUND
```
- docker build -t apache_with_code:v8 -f Dockerfile  --build-arg user=ricardo --build-arg httpd_package=httpd .

### CMD

CMD is like the active terminal running in the container. If it is closed somehow the container will stop.

- vi Dockfile3
```Dockerfile
FROM centos
ARG user=application //setting default value
ARG httpd_package

RUN yum -y install $httpd_package unzip
LABEL maintainer=Ricardo
LABEL vendor=companyB
ENV HTML beginner-html-site-styled
WORKDIR /var/www/html/ (changing directory to the specified in centos)
ADD https://github.com/$HTML/index.zip ./code.zip
RUN unzip code.zip && mv $HTML/* . (moving contents of the unziped folder to the apache root directory.) && echo $HTML > ./env.html (. means current directory)
RUN useradd $user && chown $user:$user /var/www/html -R
USER $user
RUN rm -rf code.zip $HTML-gh-pages/
USER root
#CMD apachectl -DFOREGROUND
```

The container will not start and will exit immediately.
- docker ps --all
-  docker rm -f apache10
- docker run -d -ti --name apache10 -p 9091:80 apache_with_code:v10
- docker exec -ti apache10 bash
- /usr/sbin/httpd -DFOREGROUND (might not work)
- apachectl -DFOREGROUND
- apachectl -DBACKGROUND
- docker rm -f apache10
- vi Dockerfile3
```Dockerfile
FROM centos
ARG user=application //setting default value
ARG httpd_package

RUN yum -y install $httpd_package unzip
LABEL maintainer=Ricardo
LABEL vendor=companyB
ENV HTML beginner-html-site-styled
WORKDIR /var/www/html/ (changing directory to the specified in centos)
ADD https://github.com/$HTML/index.zip ./code.zip
RUN unzip code.zip && mv $HTML/* . (moving contents of the unziped folder to the apache root directory.) && echo $HTML > ./env.html (. means current directory)
RUN useradd $user && chown $user:$user /var/www/html -R
USER $user
RUN rm -rf code.zip $HTML-gh-pages/
USER root
CMD apachectl -DBACKGROUND
```
- docker run -d -ti --name apache10 -p 9091:80 apache_with_code:v10
- docker ps -a (the process is running in the background but docker thinks it is dead so it exits.)
- docker logs apache10
- docker rm -f apache10
- vi Dockerfile3
```Dockerfile
CMD apachectl -DFOREGROUND
```
- docker ps --no-trunc
- docker exec -ti apache10 bash
- ps aux
- pkill -9 httpd (process died so the container also died.)
- docker logs apache11
- docker logs -f apache11 (foreground live logs)

We can run custom scripts "cmd.sh" CMD /cmd.sh.