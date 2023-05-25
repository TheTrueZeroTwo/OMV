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



    If using NVidia GPU
- [ ] Install Nvidia container Toolbox (https://forum.openmediavault.org/index.php?thread/38013-howto-nvidia-hardware-transcoding-on-omv-5-in-a-plex-docker-container/&postID=313378#post313378)
```
apt-get install module-assistant
```
```
sudo m-a prepare
```
```
nano /etc/apt/sources.list
```
add if ***not*** there
# Debian Bullseye
deb http://deb.debian.org/debian/ bullseye main contrib non-free
```
apt update
```
```apt install nvidia-driver firmware-misc-nonfree
```
If there are no errors
```
apt install nvidia-xconfig
```
```
sudo nvidia-xconfig
```
```
echo 'GRUB_CMDLINE_LINUX=systemd.unified_cgroup_hierarchy=false' > /etc/default/grub.d/cgroup.cfg
```
```
update-grub
```
```
reboot now
```

```
apt install nvidia-smi
```
```
nvidia-smi
```
```
apt install curl
```
```sh
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
      && curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
      && curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list | \
            sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
            sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```
```
sudo apt-get update
```
```
apt install nvidia-docker2
```
```
apt install libnvidia-encode1
```
```
apt install nvidia-container-runtime
```
```
sudo systemctl restart docker
```
open Jellyfin
in the tab "Runtime & Ressources":
change the "Runtime" Value from runc to nvidia !!!
add enviriment variable
NVIDIA_VISIBLE_DEVICES=all
```
apt install git
```
```
git clone https://github.com/keylase/nvidia-patch.git nvidia-patch
```
```
cd nvidia-patch
```
```
bash ./patch.sh
```
