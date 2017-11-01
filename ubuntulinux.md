![](https://d207aa93qlcgug.cloudfront.net/dockerized-0.23.1/img/explore_repos/official_ubuntu-h.png)

![](http://manpages.ubuntu.com/assets/light/images/footer_logo.png)

```
shell> lsb_release -a
```

```
No LSB modules are available.
Distributor ID:    Ubuntu
Description:    Ubuntu 10.04.4 LTS
Release:    10.04
Codename:    lucid
```

```
No LSB modules are available.
Distributor ID:    Ubuntu
Description:    Ubuntu 12.04.5 LTS
Release:    12.04
Codename:    precise
```

```
No LSB modules are available.
Distributor ID:    Ubuntu
Description:    Ubuntu 12.10
Release:    12.10
Codename:    quantal
```

```
No LSB modules are available.
Distributor ID:    Ubuntu
Description:    Ubuntu 14.04 LTS
Release:    14.04
Codename:    trusty
```

```
No LSB modules are available.
Distributor ID:    Ubuntu
Description:    Ubuntu 16.04.1 LTS
Release:    16.04
Codename:    xenial
```

```
shell> lsb_release -si
```

```
Ubuntu
```

---

```
New release '14.04.3 LTS' available.
Run 'do-release-upgrade' to upgrade to it.

New release '14.04.2 LTS' available.
Run 'do-release-upgrade' to upgrade to it.
```

`/etc/update-manager/release-upgrades`

```
#Prompt=lts
Prompt=never
```

#### :books: 參考網站：

* [https://help.ubuntu.com/lts/serverguide/installing-upgrading.html](https://help.ubuntu.com/lts/serverguide/installing-upgrading.html)

```
shell> sudo sed -i 's/Prompt=.*/Prompt=never/' /etc/update-manager/release-upgrades
shell> sudo perl -pi -e 's/Prompt=.*/Prompt=never/' /etc/update-manager/release-upgrades
shell> sudo rm /var/lib/update-notifier/release-upgrade-available
```

```
shell> sudo sed -i '/^/s/^/#/" /etc/apt/sources.list
```

```
shell> apt-get --version
shell> apt-get -o Acquire::ForceIPv4=true update
```

`apt.conf`

```
Acquire::ForceIPv4 "true";

Acquire
{
  ForceIPv4 "true";
  Proxy "http://dockerhost:3142";
};

Acquire::http
{
  Proxy "http://dockerhost:3142";
};
```

#### :books: 參考網站：

* [apt.conf](http://manpages.ubuntu.com/manpages/raring/man5/apt.conf.5.html)
* [apt-cacher-ng](https://docs.docker.com/engine/examples/apt-cacher-ng/)

```
Acquire::http::Timeout "10";
Acquire::ftp::Timeout "10";
```

---

```
shell> /etc/network/interfaces
```

```
# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
auto eth0
# iface eth0 inet dhcp
iface eth0 inet static
               address 192.168.1.3
               netmask 255.255.255.0
               gateway 192.168.1.1
               dns-nameservers 192.168.1.254 8.8.8.8
               dns-search foo.org bar.com
               post-up arp -f
```

---

`/etc/ethers`

```
shell> vim /etc/ethers
```

```
90:94:e4:07:df:14 192.168.42.1
b8:27:eb:6e:88:a3 192.168.42.25
00:0c:29:b7:22:3c 192.168.42.31
00:0c:29:d1:41:b0 192.168.42.32
f0:b4:29:73:2d:05 192.168.42.56
00:10:18:03:92:7a 192.168.42.200
00:0e:38:5e:f3:c0 192.168.42.253
```

---

`vlan`

```
shell> modprobe 8021q
shell> apt-get install vlan
shell> vconfig add eth0 222 # 222 vlan-id
shell> ifconfig eth0.222 up
shell> ifconfig eth0.222 192.168.2.3 netmask 255.255.255.0

shell> ip addr add 192.168.2.3/24 dev eth0.222
```

```
shell> /etc/modules
```

```
8021q
```

```
auto eth0.222
iface eth0.222 inet static
            vlan-raw-device eth0
            address 192.168.2.3
            netmask 255.255.255.0
```

```
shell> vconfig rem eth0.222
```

---

```
shell> vim /etc/sysctl.conf
```

```ini
[...]
net.ipv6.conf.all.disable_ipv6 = 1
```

```
shell> sysctl --system
```

```
shell> reboot
```

```
shell> vim /etc/default/grub
```

```ini
[...]
GRUB_CMDLINE_LINUX="ipv6.disable=1"
GRUB_CMDLINE_LINUX="net.ifnames=0 biosdevname=0"

[...]
```

```
shell> update-grub
shell> reboot
```

```
shell> grub-mkconfig -o /boot/grub/grub.cfg
```

---

```
shell> cat /proc/sys/fs/file-nr
shell> cat /proc/sys/fs/file-max

shell> cat /proc/sys/fs/inode-nr
```

---

`unattended-upgrades`

```
shell> apt-get install unattended-upgrades
shell> dpkg-reconfigure unattended-upgrades
shell> dpkg-reconfigure -plow unattended-upgrades
```

![Imgur](http://i.imgur.com/kFM6tSt.png)  
![Imgur](http://i.imgur.com/GbfS21y.png)

`/etc/apt/apt.conf.d/20auto-upgrades`

```
APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Unattended-Upgrade "1";
```

#### :books: 參考網站：

* [automatic-updates](https://help.ubuntu.com/lts/serverguide/automatic-updates.html)
* [dpkg-reconfigure](http://manpages.ubuntu.com/manpages/saucy/man8/dpkg-reconfigure.8.html)
* [http://docs.openstack.org/icehouse/install-guide/install/apt-debian/content/debian\_packages.html](http://docs.openstack.org/icehouse/install-guide/install/apt-debian/content/debian_packages.html)

---

```
shell> apt-get install lvm2
shell> pvscan
```

---

![](http://i.imgur.com/ykaLJcH.png)

`F6`

![](http://i.imgur.com/BQAKJ1j.png)

ks=[http://172.16.7.15/ks.cfg](http://172.16.7.15/ks.cfg)

```
#System language
lang en_US

#Language modules to install
langsupport en_US

#System keyboard
keyboard us

#System timezone
#timezone America/New_York
timezone Asia/Taipei

#Root password
rootpw --disabled

#Initial user (user with sudo capabilities) 
user ubuntu --fullname "Ubuntu User" --password root4me2

#Reboot after installation
reboot

#Use text mode install
text

#Install OS instead of upgrade
install

#Installation media
cdrom
#nfs --server=server.com --dir=/path/to/ubuntu/

#System bootloader configuration
bootloader --location=mbr 

#Clear the Master Boot Record
zerombr yes

#Partition clearing information
clearpart --all --initlabel 

#Basic disk partition
part / --fstype ext4 --size 1 --grow --asprimary 
part swap --size 1024 
part /boot --fstype ext4 --size 256 --asprimary 

#Advanced partition
#part /boot --fstype=ext4 --size=500 --asprimary
#part pv.aQcByA-UM0N-siuB-Y96L-rmd3-n6vz-NMo8Vr --grow --size=1
#volgroup vg_mygroup --pesize=4096 pv.aQcByA-UM0N-siuB-Y96L-rmd3-n6vz-NMo8Vr
#logvol / --fstype=ext4 --name=lv_root --vgname=vg_mygroup --grow --size=10240 --maxsize=20480
#logvol swap --name=lv_swap --vgname=vg_mygroup --grow --size=1024 --maxsize=8192

#System authorization infomation
auth  --useshadow  --enablemd5 

#Network information
network --bootproto=dhcp --device=eth0

#Firewall configuration
firewall --disabled --trust=eth0 --ssh 

#Do not configure the X Window System
skipx

user joe --fullname "Joe User" --password iamjoe
```

```
ks=cdrom:/ks.cfg
ks=floppy
ks=nfs:<server>:/<path>
ks=http://<server>/<path>
ks=floppy:/<path>
ks=cdrom:/<path>
```

#### :books: 參考網站：

* [ch04s06](https://help.ubuntu.com/lts/installation-guide/i386/ch04s06.html)
* [s1-kickstart2-startinginstall](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/3/html/System_Administration_Guide/s1-kickstart2-startinginstall.html)

```
linux ks=floppy
```

#### :books: 參考網站：

* [10.2. Changing Network Kernel Settings](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/5/html/Tuning_and_Optimizing_Red_Hat_Enterprise_Linux_for_Oracle_9i_and_10g_Databases/sect-Oracle_9i_and_10g_Tuning_Guide-Adjusting_Network_Settings-Changing_Network_Kernel_Settings.html)
* [Starting a Kickstart Installation](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/3/html/System_Administration_Guide/s1-kickstart2-startinginstall.html)

---

```
shell> apt-get install lib32z1 lib32ncurses5 lib32bz2-1.0
```

```
shell> uname -m
x86_64
```

```
shell> debconf-set-selections
shell> debconf-get-selections
```

---

`free - Display amount of free and used memory in the system`

```
shell> free -b
shell> free -m
shell> free -g
shell> free -t
shell> free -s 1
shell> free
```

```
              total        used        free      shared  buff/cache   available
Mem:         500308       54208      117568        6612      328532      416220
Swap:             0           0           0
```

```
free   = total  - used  - buff/cache
117568 = 500308 - 54208 - 328532

free   + buff/cache = total  - used
117568 + 328532     = 500308 - 54208
```

```
             total       used       free     shared    buffers     cached
Mem:       4049592    1044268    3005324          0      55864     768244
-/+ buffers/cache:     220160    3829432
Swap:      2095100          0    2095100
```

```
used    - (buffers + cached)
1044268 - (55864   + 768244) = 220160

free    + buffers + cached
3005324 + 55864   + 768244 = 3829432
```

```
shell> cat /proc/meminfo

MemTotal:         500308 kB
MemFree:          117504 kB
MemAvailable:     416268 kB
Buffers:           87608 kB
Cached:           177412 kB
SwapCached:            0 kB
Active:           204196 kB
Inactive:          95904 kB
Active(anon):      35640 kB
Inactive(anon):     6052 kB
Active(file):     168556 kB
Inactive(file):    89852 kB
Unevictable:           0 kB
Mlocked:               0 kB
SwapTotal:             0 kB
SwapFree:              0 kB
Dirty:                 0 kB
Writeback:             0 kB
AnonPages:         35108 kB
Mapped:            16312 kB
Shmem:              6612 kB
Slab:              63628 kB
SReclaimable:      50892 kB
SUnreclaim:        12736 kB
KernelStack:        1904 kB
PageTables:         3444 kB
NFS_Unstable:          0 kB
Bounce:                0 kB
WritebackTmp:          0 kB
CommitLimit:      250152 kB
Committed_AS:     141116 kB
VmallocTotal:   34359738367 kB
VmallocUsed:           0 kB
VmallocChunk:          0 kB
HardwareCorrupted:     0 kB
AnonHugePages:         0 kB
CmaTotal:              0 kB
CmaFree:               0 kB
HugePages_Total:       0
HugePages_Free:        0
HugePages_Rsvd:        0
HugePages_Surp:        0
Hugepagesize:       2048 kB
DirectMap4k:       77816 kB
DirectMap2M:      446464 kB
DirectMap1G:           0 kB
```

---

```
shell> tracepath amazon.com
```

---

```
shell> sudo sed -i -e 's/us.archive.ubuntu.com/old-releases.ubuntu.com/g' /etc/apt/sources.list
shell> sudo sed -i -e 's/security.ubuntu.com/old-releases.ubuntu.com/g' /etc/apt/sources.list

shell> sudo sed -i -e 's/us.archive.ubuntu.com\|security.ubuntu.com/old-releases.ubuntu.com/g' /etc/apt/sources.list
```

```
shell> do-release-upgrade
```

#### :books: 參考網站：

* [http://old-releases.ubuntu.com/releases/](http://old-releases.ubuntu.com/releases/)
* [http://us.archive.ubuntu.com/](http://us.archive.ubuntu.com/)
* [http://security.ubuntu.com/ubuntu/](http://security.ubuntu.com/ubuntu/)

---

```
shell> dhclient -r eth0
shell> dhclient eth0
```

---

`hostnamectl - Control the system hostname`

```
shell> hostnamectl status
shell> hostnamectl set-hostname name
```

```
Description:    Ubuntu 14.04.5 LTS
```

#### :books: 參考網站：

* [https://access.redhat.com/documentation/zh-CN/Red\_Hat\_Enterprise\_Linux/7/html/Networking\_Guide/sec\_Configuring\_Host\_Names\_Using\_hostnamectl.html](https://access.redhat.com/documentation/zh-CN/Red_Hat_Enterprise_Linux/7/html/Networking_Guide/sec_Configuring_Host_Names_Using_hostnamectl.html)

---

```
shell> ls --quoting-style=shell
```

---

#### :books: 參考網站：

* [Official Ubuntu Documentation](https://help.ubuntu.com/)
* [https://aws.amazon.com/tw/premiumsupport/knowledge-center/linux-static-hostname/](https://aws.amazon.com/tw/premiumsupport/knowledge-center/linux-static-hostname/)
* [vlan-interfaces](http://manpages.ubuntu.com/manpages/intrepid/man5/vlan-interfaces.5.html)
* [Setting File Handles](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/5/html/Tuning_and_Optimizing_Red_Hat_Enterprise_Linux_for_Oracle_9i_and_10g_Databases/chap-Oracle_9i_and_10g_Tuning_Guide-Setting_File_Handles.html)



