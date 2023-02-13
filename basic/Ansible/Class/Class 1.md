![[Pasted image 20230120210702.png]]
![[Pasted image 20230120210750.png]]
- Automation tool
- Configuration tool

Master node or Control node -> ansible from where i am running code
Managed host or Target node - where i am applying the changes

Ansible will be installed in linux but target hosts can be anything(router, os, switch etc) depends on ansible module.

authentication will be done in windows by winrm. 

ansible runs parallelly in multiple computers in the same time.

playground is in yml. (what to do will be kept here.)

if state is same then it will not apply changes.

push base system, pull base system.

ansible inventory file where server informations will be kept.

3000 modules in master module.

yum is installed in the ansible module if yum install is initiated then ansible will scp to send the data to ther server.

yum should be installed both in remote and in the control node.

```bash
Pre-Requisities:
	-ssh
	-sudo
	-linux installation

Ansible:
	-Control Node:
		-vm1
		-OS:RHEL8
		-VCPU: 2
		-RAM: 2GB
		-SPACE: 20GB
	-Manage Hosts:
		-vm2
			-OS: RHEL8/WINDOWS
			-VCPU: 2
			-RAM: 2GB
			-SPACE: 20GB
		-vm3

vi /etc/hosts'
```

- vi /etc/hosts
![[Pasted image 20230120230215.png]]
- for multiple names
![[Pasted image 20230120230405.png]]

