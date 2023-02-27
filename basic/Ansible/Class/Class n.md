date
hwclock
sudo
/etc/sudoers
/etc/sudoer.d/
visudo

```bash

#root
useradd alex
passwd alex
date
hwclock #bios time | bios time and os time might not be always same.


#alex
date
hwclock #doesn't show (has no permission)

#root
vi /etc/sudoers
# user MACHINE=COMMANDS
# alex ALL=/sbin/hwclock, /usr/sbin/useradd
# alvee ALL=(ALL) NOPASSWD: ALL
# %cisco ALL=(ALL) NOPASSWD: ALL #group sudo commands

visudo

#alex
#now alex can run sudo hwclock, useradd

```