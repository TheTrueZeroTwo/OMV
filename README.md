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
- [ ] Set up Certificates
- [ ] use cert for ssl
- [ ] Set time zone
- [ ] Add ZFS ZED to notifacations
- [ ] Allow Communitty maintained updates
- [ ] Update
- [ ] Enable extra interfaces if avaible
- [ ] create ZFS pool (sets of mirrored pools are prefered for large dirves for shorter rebuild time)
- [ ] Set up zfs auto snapshots
    `wget https://github.com/zfsonlinux/zfs-auto-snapshot/archive/upstream/1.2.4.tar.gz`
    `tar -xzf 1.2.4.tar.gz`
    `cd zfs-auto-snapshot-upstream-1.2.4`
    `make install`
    
- [ ] Enable FTP ***CHANGE PORT***
- [ ] Enable SSL for FTP
- [ ] Add shared folders to be shared by FTP (usaully folders that will have automation eg backups)
- [ ] Enable NFS
- [ ] Enable shared folders (client is the first 3 numbers + 0 / 24 eg 192.168.1.0/24)
- [ ] Ebable RSync if using it
- [ ] Add Modules (shared folders)
- [ ] Enable SMB/CIFS
- [ ] Add shared folders

- [ ] Enable UPS (If you don't have one I ***Highly*** reccomend to get on, Cyber Power ones are a good starting one)

- [ ] Add users (at least one for docker containers eg abc, if you don't want an account to be able to login set shell to nologin, if you dont want a acount to chage itself dissallow account modification)
- [ ] Add group with the same name

- [ ] Add Docker containers
