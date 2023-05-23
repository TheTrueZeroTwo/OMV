# OMV
Steps for creating a OMV nas that is running selfhosted docker docker containers

In between every step there should be a reboot and ctrl-shift-f5 on the webpage.
(you could get away with less rebooting but because a lot is being changed i like to make sure that i take my time.


# Step 1 Download and instalation
https://www.openmediavault.org/?page_id=77

#s tep 2 Install OMV extras
SSH into OMV using a terminal eg(ssh root@omv.local).

```sh
wget -O - https://github.com/OpenMediaVault-Plugin-Developers/packages/raw/master/install | bash
```
# step 3 Install plugins
The plugins i use are

- [ ] FTP
- [ ] KERNEL
- [ ] KVM
- [ ] NUT
- [ ] ZFS


# step 4 Install core componets

- [ ] Install Promox kernel
- [ ] Setup ZFS
- [ ] Install Docker
- [ ] Install Portainer (Setup admin account after installing as there is a timeout for it)
- [ ] 

