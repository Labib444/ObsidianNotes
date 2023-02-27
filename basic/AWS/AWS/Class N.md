- Install
- Start
- Enable
- test

Package: httpd
Service: httpd
Firewall: 80/tcp
Document: /var/www/html
cat /etc/os-release

yum repolist
cd /etc/yum.repos.d/
yum install httpd -y
rpm - q httpd
systemctl status httpd
systemctl start httpd
systemctl enable httpd

curl http://localhost
cd /var/www/html
vi index.html

echo welcome to AWS-CLOUD > index.html
ifconfig //doesn't show real ip shows private ip must see it from the aws website

must provide port access in security group inbound.
stop instance -> start instance (public ip will be changed.)
restart intance (public ip will not be changed.)

Private:
	A-Class: >> 0-127 >> 1-127 >> 127.0.0.0/8 === LoopBack >> 1-126 (8bit given) 
	B-Class:  >> 128-191 (12bit given) 172.16.0.0/12 >> 172.16.0.0-172.31.255.255
	C-Class: 192-223 (16bit given) >> 192.168.0.0/16

10.0.0.0/8 >>
N.A: 10.0.0.0 - 10.255.255.255 >> 10.0.0.1-10.255.255.254

ipcalc 172.16.0.0/16 (command in linux)

private ip > NAT > public ip



## VPC

