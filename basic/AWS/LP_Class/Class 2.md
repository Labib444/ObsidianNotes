
### What will we learn

- ssh server configuration
	- password based
	- passwordless authentication
	- configure ssh server
	- connecting with ssh server using ssh client/putty client
	- configure keybased authentication
	- connecting with ssh server using ssh client/putty client

### Package

package: openssh-server
config file: /etc/ssh/sshd_config
service: sshd
port: 22/tcp

```linux cli
#ISET
#I - install >> yum/dnf/rpm
#S - start   >> systemctl start sshd
#E - enable  >> systemctl enable sshd
#T - test    >> systemctl 
```

```bash
#check if package installed
rpm -q openssh-server
clear

yum remove openssh-server
rpm -q openssh-server

#error connection refused
#maybe ssh not configured, not installed, service not started
ssh root@serverb
ping serverb

#login to server
rpm -q openssh-server
yum repolist #status = number of packages
yum install openssh-server -y
systemctl start sshd
systemctl enable sshd

#login to client
#Permission denied (public key not configured)
ssh root@serverb

#login to servera
ssh root@serverb
#type yes
#set password

grep -v "#" /etc/sshd/sshd_config
#PermitRootLogin yes
#If it is set to no then we will not be able to login through root.
#If this line is not included in the config file then by default it is "no"

#from roo@servera
ssh root@serverb
#type password

#all kind of authentication logs
tail -f /var/log/secure

#from serverb
ls
vi/etc/sshd/sshd_config
#hash PermitRootLogin yes
systemctl reload sshd

#servera
ssh root@serverb
#type password

#serverb
tail -f /var/log/secure
#try to relogin from servera
# unhash PermitRootLogin
#relogin

useradd user1
useradd user2

echo redhat | passwd --stdin user1
echo redhat | passwd --stdin user2
systemctl restart sshd

#servera
ssh user1@serverb
#type password #allowed
ssh user2@serverb
for i in {1..100}; do useradd user$i; done

#serverb
#add AllowUsers user20
systemctl restart sshd

#Root will still not get in
	#PermitRootLogin yes
	#AllowUsers user20
#Type like this it to allow root
	#PermitRootLogin yes
	#AllowUsers user20 root

#By default ssh doesn't allow empty password even if you don't have password.


```

```bash
#servera
ls
#ssh user@ip
#ssh user@hostname
#ssl -l user hostname
#ssh -p 28 user@ip
#ssh user@ip command
#ssh user@ip <command>
#ssh user20@serverb hostname #login then run one command
#ssh hostname
#scp <source-file> <remoteuser>@<remoteip>:<remote-path>

#copy the file /root/aws from servera to temp of serverb
#copy the file /root/aws from servera to /tmp of serverb using user user20 with password redhat
scp /root/aws user20@serverb/tmp/
#type password

scp -rv db user20@serverb:/tmp # -r means recursively
#if we set cronjob then it will not work because it will ask for password
#therefore we need keybased authentication


#serverb
ls
#after running the servera code
ls
#go tmp folder and see the file has been transfered


```

```bash
#Keybased authetication

# -keypair >>
# ssh-keygen
	# private key (key)
	# public  key  (public lock)
	# client >> Key (public key, private key)
	# server >> Lock

#servera
ssh-keygen
#press enter
#again press enter no need to add pass phrase else same problem will arrive
cd /root/.ssh/
ls
#id_rsa id_rsa.pub #private #public

#serverb
grep pub /etc/ssh/sshd_config
#PubkeyAuthentication yes by default always is "yes"
#AuthorizedKeysFile .ssh/authorized_keys #This is where keys are stored.

#servera
ssh-copy-id user20@serverb
#type password

ssh user20@serverb #logged in without providing password
ls -l /etc/ssh/sshd_config
ls -l /etc/ssh/ssh_config

#ssh >> local machine /etc/ssh/ssh_config
#and root@serverb means it will check the configuration of /etc/ssh/sshd_config of serverb

vi /etc/ssh/ssh_config
#Other settings are included in /etc/ssh/ssh_config.d/*conf
ssh -i /opt/id_rsa user20@serverb
mv id_rsa myKey1
ssh -i myKey1 user20@serverb

#change key owner by chown else anyone can access it to access server.
chown labib:labib myKey1
```