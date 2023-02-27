```bash
nmcli dev status
nmcli con show
nmcli con show --active
ip addr show eth0

nmcli con add con-name eno2 type ethernet ifname eno2
nmcli con add con-name eno2 type ethernet ifname eno2 \ 
	ip4 192.168.0.5/24 gw4 192.168.0.254
```