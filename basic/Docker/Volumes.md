- Volumes in Docker allows you to persist data after a container dies
- Normal Volumes
- Bind Volumes
- Anonymous volumes

### Binding Volumes
```bash
docker run -d --name mysql -e "MYSQL_ROOT_PASSWORD=1234" mysql:5.7
docker ps
docker exec -ti mysql bash
cd /var/lib/mysql
#exit
```
```bash
cd /mnt/
sudo mkdir mysql
ls -l
pwd
ls /mnt/mysql
docker run -d -v /mnt/mysql:/var/lib/mysql --name mysql -e "MYSQL_ROOT_PASSWORD=1234" mysql:5.7
docker exec -ti mysql bash
mysql -u root -p1234
show databases
create database docker_db
exit
docker rm -f container_hash

cd /mnt/mysql
ls

```
### Normal Volumes

```bash
docker volume create volume_name
docker info | grep -i root
cd /mnt/docker
cd volumes/
ll
docker volume ls

docker run -d -v volume_name:/var/lib/mysql --name mysql -e "MYSQL_ROOT_PASSWORD=1234" mysql:5.7 
```

```bash
#location: /mnt/docker/
cd volume_name/
ll
# will show _data
cd _data # (your files are stored here)
```
### Anonymous Volumes
```bash
docker ls
docker volume rm hash_of_anonymous_volume
docker ls # (deleted)

docker run -d -v /var/lib/mysql --name mysql -e "MYSQL_ROOT_PASSWORD=1234" mysql:5.7

docker volume ls # volume name is a hash code
docker inspect container_name
```

![[Pasted image 20230226053602.png]]

```bash
docker info | grep -i root
cd /mnt/docker/volumes/
ls
cd hash_code
cd _data/ # (your data)
```
- Need to assign a flag so that the volume is  deleted.
```bash
docker rm -fv container_name #adding v in the command will delete the volume
```
- using v will delete anonymous volumes others will not be deleted.

### Dangling Volumes

```bash
docker run -dti -v /mnt --name test centos
docker ps
docker rm -fv test
docker volume ls

docker run -dti -v /mnt --name test centos
docker ps
docker rm -f test
docker volume ls

docker run -dti -v /mnt --name test2 centos
docker run -dti -v /mnt --name test3 centos
docker rm -f test2 test3

docker volume ls
# you will see 3 volumes none of them are used
docker run -dti -v /mnt --name test4 centos
docker volume ls
# now you donot know which is in use.
docker volume ls -f=dangling=true -q # f: filter
# show unused volumes
docker volume rm $(docker volume ls -f=dangling=true -q)
docker volume rm hash_code # (this volume is in use)
# we cannot delete it because it is in use.
```

- If your container dies or removed but volume is still there then mapping the volume

```bash
#giving permission
id #shows userid and groupid
chown 1000:1000 -R /mnt/jenkins/ -R #giving permission to that userid and g
```
```bash
docker exec jenkins bash -c "cat /var/jenkins_home/secrets/initialAdminPassword"
docker exec jenkins bash -c "whoami"
```

![[Recording 20230227080638.webm]]
