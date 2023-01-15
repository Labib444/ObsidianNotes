### What we will learn
- keybased access configure
- login using ssh client
- login using putty

Task: 
- aws - linux
- private key will given

```bash
hostnamectl //may always not work
cat /etc/os-release
ip a s
ip a
ifconfig //may always not work
netstat -nr //may always not work
route -n //may always not work
ip route
cat /etc/resolv.conf //checking dns
ssh-keygen -t dsa
watch ls -l .ssh //will run every 2 seconds
ssh -i /opt/myserver root@192.168.122.101 
//if your id_rsa is different named and in different folder
//use permissions 400 or 600 aws recommends to use 400.
chmod 0400 myserver //myserver is the id_rsa.pub file

vim /etc/ssh/sshd_config
#PasswordAuthentication no //turn it off if it is yes (better)
systemctl restart sshd

```
- If keybased fails then it will go to password based authentication.

```bash
putty >> private-key >> login
scp opt/myserver root@192.168.1122.1:/home/kiosk/
# .PEM .ppk
# .ppk >> Putty Private Key
# .pem >> Privacy Enchancement Mail
.pem >> ssh
.ppk >> putty // putty doesn't know about .pem format.
puttygen myserver -o myserver.ppk

```

```
User: ec2-user
ServerName: ec2-18-217-165-66.us-east-2.compute.amazonaws.com
link: ec2-user@ec2-18-217-165-66.us-east-2.compute.amazonaws.com 
PrivateKey: exam1.pem
```