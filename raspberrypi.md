
<img src="http://i.imgur.com/DBvZ6Zw.png" width="200">


`pi` 

`raspberry`

**CREATE A NEW USER**
```
shell> sudo adduser bob
```

**CHANGE YOUR PASSWORD**
```
shell> passwd
```

**REMOVE A USER'S PASSWORD**
```
shell> sudo passwd bob -d
```

**DELETE A USER**
```
shell> sudo userdel -r bob
```


**SUDOERS**
```
shell> sudo visudo
```

```
# User privilege specification
root  ALL=(ALL:ALL) ALL
bob ALL=(ALL) NOPASSWD: ALL
```

```
shell> update-alternatives --set editor /usr/bin/vim.tiny
```

#### :books: 參考網站：
- [users](https://www.raspberrypi.org/documentation/linux/usage/users.md)

---

```
shell> sudo nmap -sP 192.168.11.0/24 | awk '/^Nmap/{ipaddr=$NF}/B8:27:EB/{print ipaddr}'
```

---

```
shell> lsusb
```

```
Bus 001 Device 004: ID 0bda:8176 Realtek Semiconductor Corp. RTL8188CUS 802.11n WLAN Adapter
```

```
shell> iwconfig
shell> iwconfig wlan0
shell> iwlist wlan0 scan
shell> vim /etc/wpa_supplicant/wpa_supplicant.conf
```


```
country=GB
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
	ssid="example wpa-psk network"
	key_mgmt=WPA-PSK
	proto=WPA
	pairwise=TKIP
	group=TKIP
	psk="secret passphrase"
}

network={
	ssid="Sapido_RB-1602G3_d179e7"
	key_mgmt=WPA-PSK
	proto=WPA2
	pairwise=TKIP
	group=TKIP
	psk="12345678"
}
```

`wpa_supplicant.conf`
```
network={
	ssid="N500RD-EC38"
	key_mgmt=WPA-PSK
	psk="12345678"
}
```

---

#### :books: 參考網站：
- [downloads](http://www.raspberrypi.org/downloads/)

```
shell> unzip ~/Downloads/2016-03-18-raspbian-jessie-lite.zip
shell> diskutil list
shell> diskutil unmountDisk /dev/disk3
shell> sudo dd bs=4m if=~/Downloads/2016-03-18-raspbian-jessie-lite.img of=/dev/disk3
shell> dd if=~/Downloads/2016-03-18-raspbian-jessie-lite.img | pv | sudo dd bs=4m of=/dev/rdisk3
shell> diskutil eject /dev/disk3
```

```
shell> unzip ~/Downloads/2016-09-23-raspbian-jessie-lite.zip
shell> diskutil list
shell> diskutil unmountDisk /dev/disk3
shell> sudo dd bs=4m if=~/Downloads/2016-09-23-raspbian-jessie-lite.img of=/dev/disk3
shell> dd if=~/Downloads/2016-09-23-raspbian-jessie-lite.img | pv | sudo dd bs=4m of=/dev/rdisk3
shell> diskutil eject /dev/disk3
```

```
shell> aria2c https://downloads.raspberrypi.org/raspbian_lite_latest.torrent
shell> unzip ~/Downloads/2017-04-10-raspbian-jessie-lite.zip
shell> diskutil list
shell> diskutil unmountDisk /dev/disk3
shell> dd if=~/Downloads/2017-04-10-raspbian-jessie-lite.img | pv | sudo dd bs=4M of=/dev/rdisk3
shell> diskutil eject /dev/disk3
```

**<kbd>Control ⌃</kbd> + <kbd>T</kbd>**

```
shell> dd bs=4M if=2014-09-09-wheezy-raspbian.img of=/dev/sdd
```

---

```
shell> vcgencmd commands

commands="vcos, ap_output_control, ap_output_post_processing, vchi_test_init, vchi_test_exit, pm_set_policy, pm_get_status, pm_show_stats, pm_start_logging, pm_stop_logging, version, commands, set_vll_dir, led_control, set_backlight, set_logging, get_lcd_info, set_bus_arbiter_mode, cache_flush, otp_dump, test_result, codec_enabled, get_camera, get_mem, measure_clock, measure_volts, scaling_kernel, scaling_sharpness, get_hvs_asserts, measure_temp, get_config, hdmi_ntsc_freqs, hdmi_adjust_clock, hdmi_status_show, hvs_update_fields, pwm_speedup, force_audio, hdmi_stream_channels, hdmi_channel_map, display_power, read_ring_osc, memtest, get_rsts, schmoo, render_bar, disk_notify, inuse_notify, sus_suspend, sus_status, sus_is_enabled, sus_stop_test_thread, egl_platform_switch, mem_validate, mem_oom, mem_reloc_stats, file, vctest_memmap, vctest_start, vctest_stop, vctest_set, vctest_get"
```

```
shell> vcgencmd get_config arm_freq
shell> vcgencmd get_config int
shell> vcgencmd get_config str
shell> /opt/vc/bin/vcgencmd measure_temp
shell> /opt/vc/bin/vcgencmd version
```

```
shell> sudo apt-get update && sudo apt-get install rpi-update
shell> rpi-update
```
---
```
shell> sudo raspi-config
```
![](http://i.imgur.com/BoZvwv5.jpg)

```
shell> lsusb
shell> lshw
shell> dmesg
shell> sudo dmesg -C
```

---

```
shell> dd if=/dev/zero of=/512MiB.swap bs=512 count=1048576
shell> dd if=/dev/zero of=/1GiB.swap bs=1024 count=1048576
shell> losetup /dev/loop0 /1GiB.swap 
shell> mkswap /dev/loop0
shell> swapon /dev/loop0
shell> free -m
shell> free -h     
```

```
shell> fallocate -l 1G /1GiB.swap
mkswap /1GiB.swap
shell> swapon /1GiB.swap
shell> swapon --show
```

```
NAME       TYPE       SIZE USED PRIO
/dev/sda5  partition 1022M 2.3M   -1
/1GiB.swap file      1024M   0B   -2
```

```
shell> swapoff /1GiB.swap
```

---

![](http://i.imgur.com/tahN41e.png)

```
shell> wget http://archlinuxarm.org/os/ArchLinuxARM-rpi-latest.tar.gz
shell> bsdtar -xpf ArchLinuxARM-rpi-latest.tar.gz
```

---

### 安裝  
```
shell> sudo apt-get install vim-nox
```

---

```
shell> echo 1 > /proc/sys/net/ipv6/conf/eth0/disable_ipv6
```

```
shell> echo 1 > /proc/sys/net/ipv6/conf/all/disable_ipv6
```


``` 
shell> vim /etc/default/grub
```

```ini
[...]
GRUB_CMDLINE_LINUX="ipv6.disable=1"
[...]
```

```
shell> update-grub
shell> reboot
```

---

```
shell> uname -m
```

```
armv7l
```

```
shell> curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.29.0/install.sh | bash
```

```
http://nodejs.org/dist/v4.2.1/
http://nodejs.org/dist/v4.2.1/node-v4.2.1-linux-armv7l.tar.gz
```
---

```
shell> lolcat
```

---


**dphys-swapfile - Autogenerate and use a swap file**


```
shell> sudo apt-get install dphys-swapfile

shell> vim /etc/dphys-swapfile
shell> service dphys-swapfile stop
shell> service dphys-swapfile start
```

```
CONF_SWAPFILE=/var/swap
CONF_SWAPSIZE=2048
```

#### :books: 參考網站：
- [dphys-swapfile](http://manpages.ubuntu.com/manpages/xenial/man8/dphys-swapfile.8.html)

---

```
shell> iwlist scan 

```

```
[   76.978192] usb 1-1.3: new high-speed USB device number 5 using dwc_otg
[   77.079273] usb 1-1.3: New USB device found, idVendor=2001, idProduct=3315
[   77.079299] usb 1-1.3: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[   77.079317] usb 1-1.3: Product: D-Link Wireless Adapter
[   77.079332] usb 1-1.3: Manufacturer: Realtek
[   77.079347] usb 1-1.3: SerialNumber: 123456

Bus 001 Device 005: ID 0bda:8176 Realtek Semiconductor Corp. RTL8188CUS 802.11n WLAN Adapter

[  625.155805] usb 1-1.2: new high-speed USB device number 5 using dwc_otg
[  625.257403] usb 1-1.2: New USB device found, idVendor=0bda, idProduct=8176
[  625.257432] usb 1-1.2: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[  625.257449] usb 1-1.2: Manufacturer: Realtek
[  625.257465] usb 1-1.2: SerialNumber: 00e04c000001
[  625.483819] usbcore: registered new interface driver rtl8192cu

Bus 001 Device 004: ID 2001:3314 D-Link Corp. 

[   27.586281] usb 1-1.4: new high-speed USB device number 4 using dwc_otg
[   27.687656] usb 1-1.4: New USB device found, idVendor=2001, idProduct=3314
[   27.687683] usb 1-1.4: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[   27.687701] usb 1-1.4: Product: 802.11n WLAN Adapter
[   27.687717] usb 1-1.4: Manufacturer: Realtek
[   27.687732] usb 1-1.4: SerialNumber: 00e04c000001
```

`ndiswrapper-common - Common scripts required to use the utilities for ndiswrapper`
`ndiswrapper-dkms - Source for the ndiswrapper Linux kernel module (DKMS)`
`ndiswrapper-source - Source for the ndiswrapper Linux kernel module`

```
ndiswrapper-common
ndiswrapper

ndiswrapper-common ndiswrapper-dkms

modprobe ndiswrapper
```


---

`timedatectl - Control the system time and date`

```
shell> timedatectl
shell> timedatectl list-timezones
shell> timedatectl set-timezone Asia/Taipei
shell> timedatectl set-ntp true
```

```
shell> sudo apt-get install ntpstat
shell> ntpstat
```

---

`hciconfig - configure Bluetooth devices`

```
shell> hciconfig
```

#### :books: 參考網站：
- [dphys-swapfile](http://manpages.ubuntu.com/manpages/trusty/man8/dphys-swapfile.8.html)

---

`EDIMAX EW-7811Un無線網路卡`

<img src="http://www.edimax.com.tw/edimax/mw/cufiles/images/products/pics/ew-7811un/big/EW-7811Un_01_1000x1000.jpg" width="100">

<img src="http://www.edimax.com.tw/edimax/mw/cufiles/images/products/pics/ew-7811un/download/EW-7811Un_03_side_1000x1000.png" width="100">

<img src="http://www.edimax.com.tw/edimax/mw/cufiles/images/products/pics/ew-7811un/big/EW-7811Un_02_top_1000x1000.jpg" width="100">


<img src="http://www.edimax.com.tw/edimax/mw/cufiles/images/products/pics/ew-7811un/big/EW-7811Un_05_Raspberry_Pi.jpg" width="200">


#### :books: 參考網站：
- [ew-7811un](http://www.edimax.com.tw/edimax/merchandise/merchandise_detail/data/edimax/tw/wireless_adapters_n150/ew-7811un/)

---
#### :books: 參考網站：

- [raspi-config](http://www.raspberrypi.org/documentation/configuration/raspi-config.md)
- [config-txt](http://www.raspberrypi.org/documentation/configuration/config-txt.md)
- [apt](http://www.raspberrypi.org/documentation/linux/software/apt.md)
- [losetup](http://manpages.ubuntu.com/manpages/precise/man8/losetup.8.html)
- [archlinuxarm](http://archlinuxarm.org/platforms/armv6/raspberry-pi)
- [archlinuxarm](http://archlinuxarm.org/support/reinstallation)

---

### rpi-raspbian {#rpi-raspbian}
[](!https://resin-packages.s3.amazonaws.com/logo/large_resin_logo.png)

```
shell> wget -qO- https://get.docker.com/ | sh
shell> docker pull resin/rpi-raspbian
shell> docker run --rm -i -t resin/rpi-raspbian
```

```
FROM resin/rpi-raspbian:wheezy-20170628
FROM resin/rpi-raspbian:latest

MAINTAINER Timmy Lo

wget https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-arm.zip
unzip /path/to/ngrok.zip
```

#### :books: 參考網站：
- https://hub.docker.com/r/resin/rpi-raspbian/
- https://hub.docker.com/r/resin/rpi-raspbian/tags/

---

`M2`

<img src="http://i.imgur.com/C8jRmHu.png" width="200">

<img src="http://i.imgur.com/AtIVN6N.jpg" width="200">

<img src="http://i.imgur.com/T9fD1Nj.png" width="200">

