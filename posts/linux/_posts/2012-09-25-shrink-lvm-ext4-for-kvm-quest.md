---
layout: post
title: Shrink LVM/ext4 partion
---

# Shrink /var inside a KVM quest that uses LVM and ext4.

## Login to the host to get the vnc id.

    [root@vh01-sc ~]# virsh list
     Id    Name                           State
    ----------------------------------------------------
     40    ntp-sc                         running

    [root@vh01-sc ~]# virsh vncdisplay 40
    :11


## Connect VNC to the guest.

    MacBook-Pro:~ arlukin$ ssh root@vh01-sc -L 5900:localhost:5911

## Use your vncclient to connect to localhost:5900

## Reboot the guest.

    reboot

## At the GRUB splash screen at boot time, press any key to enter the GRUB interactive menu.

## Select the configuratio you like to boot and type a to append the line.

## Go to the end of the line and type *single* as a separate word. Press Enter to exit edit mode.

## Wait for server to boot and login.

## If you for example like to shrink the /var partion, first unmount the partion and all it's 'subpartions'. To do so, first list all partions.

        [root@ntp-av ~]# df -h
        Filesystem            Size  Used Avail Use% Mounted on
        /dev/mapper/VolGroup00-root
                              4.0G  1.3G  2.6G  33% /
        tmpfs                 4.9G     0  4.9G   0% /dev/shm
        /dev/vda1              97M   27M   66M  29% /boot
        /dev/mapper/VolGroup00-home
                             1008M   34M  924M   4% /home
        /dev/mapper/VolGroup00-tmp
                             1008M   34M  924M   4% /tmp
        /dev/mapper/VolGroup00-var
                               94G   39G   50G  44% /var
        /dev/mapper/VolGroup00-varlog
                              4.0G  138M  3.7G   4% /var/log
        /dev/mapper/VolGroup00-vartmp
                             1008M   34M  924M   4% /var/tmp

## Then unmount them

    [root@ntp-av ~]# /var/log
    [root@ntp-av ~]# /var/tmp
    [root@ntp-av ~]# /var


## Before you can shrink the partion, it must be check for errors.

    e2fsck -f /dev/mapper/VolGroup00-var

## Shrink ext4 and then the LV to the desired size

    resize2fs -p /dev/mapper/VolGroup00-var 40G
    lvreduce -L 40G /dev/mapper/VolGroup00-var

## Before continuing, run e2fsck. If it bails because the partition is too small, don't panic! The LV can still be extended with lvextend until e2fsck succeeds, e.g.: lvextend -L +1G /dev/mapper/VolGroup00-var

    e2fsck -f /dev/mapper/VolGroup00-var

## Resize the filesystem to match the LVs size.

    resize2fs -p /dev/mapper/VolGroup00-var

## Check if the resize did go without any errors.

    e2fsck -f /dev/mapper/VolGroup00-var

## Reboot the machine

    reboot
