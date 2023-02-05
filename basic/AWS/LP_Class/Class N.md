rpm -q ansible
ansible --version

servera > serverb > serverc > serverb > workstation > bastion

cd /etc/ansible/
vi ansible.cfg

ansible all --list-hosts


servera

[web]
serverc
serverd

[ftp]
serverd

ansible all --list-hosts
ansible ftp --list-hosts
ansible ungrouped --list-hosts
ansible pavel --list-hosts
```bash
ansible '*' -list-host
```

```bash
#workstation
ansible (press tab)
ansible-inventory --list-hosts
ansible-inventory --list
ansible-doc 
grep -i win ansi-modlist.txt | grep win_reboot Reboot a windows machine
ansible-doc win_reboot
ansible-doc -l | grep yum
ansible-doc user
ansible-doc command
ansible-doc -l | wc -l

ansible all -m ping #here m is module
vi ansible.cfg 
# setting remote_user=user200 #line number 107 approx
ansible all -m ping -k #if all same password then all success
#ping: pong means success

```