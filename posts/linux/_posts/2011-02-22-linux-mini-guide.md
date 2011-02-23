---
layout: post
title: Linux mini guide
---

### Find all files except .svn that includes @todo

    find . -name .svn -prune -o -type f -exec grep -Hn “@todo” {} \;

### Find all files including the ip 10.100.50.3 and delete them.

    find -exec grep 10.100.50.3 {} \;|awk ‘{print $3}’ |xargs -L 1 rm

### Setup ssh login without password

If you like to login from computer A to computer B with ssh, without
using any password, follow the below script code. This is useful if you
have some automated scripts that do things on other computers.

Generate authentication keys, login to computer A as user a. Do not
enter a passphrase:

    a@A:~> ssh-keygen -t rsa
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/a/.ssh/id_rsa):
    Created directory '/home/a/.ssh'.
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    Your identification has been saved in /home/a/.ssh/id_rsa.
    Your public key has been saved in /home/a/.ssh/id_rsa.pub.
    The key fingerprint is:
    3e:4f:05:79:3a:9f:96:7c:3b:ad:e9:58:37:bc:37:e4 a@A
    a@a:~$ ssh b@b 'mkdir -p .;chmod 700 .ssh; \
    touch .ssh/authorized_keys;chmod 640 .ssh/authorized_keys'
    b@B's password:
    a@A:~> cat .ssh/id_rsa.pub | ssh b@B 'cat >> .ssh/authorized_keys'
    b@B's password:

Now it’s possible to login to B from A with user a and no password.

    a@A:~> ssh b@B hostname
    B

### Setup nfs

Click [here](https://help.ubuntu.com/community/SettingUpNFSHowTo)
or [here](http://www.ubuntugeek.com/nfs-server-and-client-configuration-in-ubuntu.html)
for external link.

### Find all ips on a network
    nmap -v -sP 192.168.0.*

### View the arp cache (ip vs mac)
arp -na

### Enable ip-forarding
sysctl -w net.ipv4.ip_forward=1

### Setup dar backup

Click [here](http://dar.linux.free.fr/doc/mini-howto/index.html) for a
mini howto about dar backups.
Click [here](http://gradha.sdf-eu.org/dar_scripts/dar_backups.sh) to
download a dar backup script
or [here](https://github.com/arlukin/home/blob/master/bin/dar_backups.sh)
to download my version.

### Linux commands

Click [here](http://www.pixelbeat.org/cmdline.html) or
[here](http://ss64.com/bash/) for mini linux command howto.

### Temp on ati GPU

    aticonfig –odgt | grep Temp | cut -b 43-47
    for (( i=0;i<10;i++ )) ;
    do
      echo -n `date`
      aticonfig –odgt | grep Temp| cut -b 42-47;
      sleep 1;
    done

### Test network speed

    On server: ./iperf -s -K 1M
    On Client: ./iperf -c 10.0.1.100 -i 1

### Centos kernel rescue

If the kernel is damaged, or as in my case you like to move a hard drive
from an intel to an amd computer. You need to rebuild/reinstall the kernel.
    boot the centos installation cd/dvd
    Type “linux rescue” in the boot menu
    Click next, next, next (connect to the network)
    When in the shell type
    chroot /mnt/sysimage
    yum install kernel
    reboot

### Create and mount an lvm logical volume

    vgcreate vg_data /dev/hdd
    lvcreate -n VolData -L 50G vg_data
    mke2fs -j /dev/vg_data/VolData
    mount /dev/vg_data/VolData /opt
    echo “/dev/vg_data/VolData  /opt  ext3  defaults  1 2″ >> /etc/fstab
    Execute a program periodically, showing output fullscreen
    watch “ps aux|grep mysql”