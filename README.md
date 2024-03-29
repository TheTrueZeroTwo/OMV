# OMV
Steps for creating a OMV nas that is running selfhosted docker docker containers

In between every step there should be a reboot and ctrl-shift-f5 on the webpage.
(you could get away with less rebooting but because a lot is being changed i like to make sure that i take my time.


# Step 1 Download and instalation

- [ ] If offered a choice, chose the text install.
- [ ] Boot Menu: Select Install
- [ ] Select a Language: (As needed)
- [ ] Select your Location: (As appropriate.)
- [ ] Configure the Keyboard: (Select as appropriate)
- [ ] Configure the Network (eg. OMV)
- [ ] Configure the Network (local)
- [ ] Follow the on screen guidance for setting the root password.
- [ ] Select your time zone.
- [ ] Select Your Main/System Drive
- [ ] window asks for confirmation of partition selections. Select Yes.
- [ ] picking your country or the closest location to your country would be the logical choice.
- [ ] The default choice is usually best.
- [ ] In most cases this entry will be blank.
- [ ] (IF Prompted) Install the GRUB Boot Loader on a Hard Disk.
- [ ] Finish the Installation

In a web browser's address bar, type in the IP address provided by the first boot screen:
Set the language of your choice.
The user name is admin and default password is openmediavault



#s tep 2 Install OMV extras

- [ ] Enter SSH
- [ ] Enable [X]
- [ ] Change Port
[X] Permit root login
Specifies whether it is allowed to login as superuser.
[X] Password authentication
Enable keyboard-interactive authentication.
[X] Public key authentication
Enable public key authentication.


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
- [ ] Expand Storage
- [ ] Enter Disks
- [ ] Make sure all your drives are detected

- [ ] Expand zfs
- [ ] Enter Pools
- [ ] Make a pool
- [ ] Name=pool, https://www.raidz-calculator.com/raidz-types-reference.aspx, Select all devices
- [ ] Save

- [ ] Enter Shared Folders
- [ ] Add (Case sensitive), Backup, Media, Storage, Isos, Volumes, Print, Docker

- [ ] Install Promox kernel

- [ ] Expand system
- [ ] Enter Workbench
- [ ] Set Aoutlogout, Enable SSL (click the + to create a new cert)

- [ ] Enter Date and Time
- [ ] Use a NTP server
- [ ] pool.ntp.org, [your gatewayip]/24

- [ ] Expand notifications
- [ ] Enter settings
- [ ] Smtp server=smtp.gmail.com, Port=587, Encryption=Starttls, email=[your email], auth required [x], username=[your email to login], passowrd=[your google app password], Revipient email=[where the email is sent to]

- [ ] Enter events
- [ ] Select the events you want notifications for

- [ ] Enter Monitoring
- [ ] Make sure it is checked [X]

- [ ] Enter Backup
- [ ] select settings
- [ ] Setup Backup of your main/system drive

- [ ] Enter Kernel
- [ ] Maksure the Linux x.x.xx - pve is default

- [ ] enter omv extras
- [ ] make sre docker repo is checked [X]


- [ ] Expand network
- [ ] enter Interfaces
- [ ] Set static ip and enable WOL, Disable ipv6 if you want

- [ ] Expand Services
- [ ] Enter Compose
- [ ] Enter Settings
- [ ] Install docker
- [ ] Compose Files
Docker [on /dev/sdf2, docker/]
Shared folder
Location of compose files
abc
Owner of directories and files *
abc
Group of directories and files *
Administrator - read/write, Users - read/write, Others - read-only
- [ ] Enter Files
- [ ] Add
```yml
---
version: "3"
services:
  portainer:
    image: portainer/portainer-ce:latest
    container_name: portainer
    restart: unless-stopped
    security_opt:
      - no-new-privileges:true
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - /var/lib/docker/volumes/portainer_data:/data
    ports:
      - 9000:9000
volumes:
  portainer-data:
    external: true
    name: portainer_data
```
- [ ] Make sure it is up
      
- [ ] Enter Services
- [ ] Make sure Portainer is up and running
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

- [ ] Enter SMB/CIFS (or your preferd shaing protocal)
- [ ] Enter Settings
- [ ] Enable
- [ ] Enter Shares
- [ ] Select all Shares exept Isos, Volumes



- [ ] Enable UPS (If you don't have one I ***Highly*** reccomend to get on, Cyber Power ones are a good starting one)
- [ ] Enable and setup according to the UPS Specification
- [ ] Test the settings

- [ ] Add users (at least one for docker containers eg abc, if you don't want an account to be able to login set shell to nologin, if you dont want a acount to chage itself dissallow account modification)
- [ ] Add group with the same name

- [ ] Add Docker containers in portainer eg. Stacks



    ## If using NVidia GPU ##
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
add if ***not*** there add

```sh
# Debian Bullseye
deb http://deb.debian.org/debian/ bullseye main contrib non-free
```
To save and exit press ctrl+x then y
```
apt update
```
```
apt install nvidia-driver firmware-misc-nonfree
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


# Helpful Links #
https://wiki.omv-extras.org/

https://wiki.omv-extras.org/doku.php?id=omv6:new_user_guide

https://www.openmediavault.org/?page_id=77
