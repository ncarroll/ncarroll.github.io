---
layout: post
title:  "Setting up VMWare shared folders on Ubuntu guest"
date:   2012-09-18 15:16:00
---

For some reason Ubuntu 12.04 is unable to complete the VMware tools automatically. I discovered this by checking if the hgfs module was loaded using the following command.


$ lsmod | grep vmhgfs

And got a bunch of errors when trying to load the module.


$ modprobe vmhgfs

To install VMware tools follow the below instructions.

In VMware Fusion click on Virtual Machine -> Reinstall VMWare Tools. This will attach the VMware tools ISO to a CDROM on the guest VM. Mounting the CDROM doesn’t seem to automatically work on my set up so I had to manually mount it with the following command.


$ sudo mount /dev/cdrom1 /media/cdrom

Tip: You might need to try /dev/cdrom if /dev/cdrom1 doesn’t work for you.

Next you need to install linux headers and build essentials. Use the following commands to install these packages.


$ sudo apt-get install build-essential
$ sudo apt-get install linux-headers-`uname -r`

Now copy the VMwareTools tar ball to your working directory, and install it.


$ cp /media/cdrom/VMwareTools-.tar.gz .
$ tar zxvf VMwareTools-.tar.gz
$ cd vmware-tools-distrib
$ sudo ./vmware-install.pl

This should initiate the install and configuration settings. I just accepted all the default values.

Now you should be able to access your host’s shared folder at /mnt/hgfs.

