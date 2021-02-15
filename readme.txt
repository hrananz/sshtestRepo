*******************************************************
Firewall:
*******************************************************
#! /bin/bash
iptables -P INPUT ACCEPT
iptables -F
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
iptables -A INPUT -i lo -j ACCEPT
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
iptables -A INPUT -i wlan1 -s 192.168.1.0/24 -j ACCEPT
iptables -A INPUT -i wlan1 -s 119.47.0.0/255.255.0.0 -j ACCEPT
iptables -A INPUT -i wlan1 -s 172.16.252.13 -j ACCEPT
iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT ACCEPT
iptables -L -v


# To save ip tables for reboot:
/sbin/service iptables save

*******************************************************
Network restart:
*******************************************************
#! /bin/bash

echo ${1}

if ! ping -q -c 1 -w 2 ${1} > /dev/null; then
echo "Restart nic"
else
echo ping is working
fi 

*******************************************************
#To find ubuntu version:
*******************************************************
lsb_release -a

*******************************************************
#To do release upgrade of Ubuntu:
*******************************************************
do-release-upgrade


*******************************************************
#To run scripts at start up:
*******************************************************
Create a file like local in the /etc/init.d/
> touch /etc/init.d/local
Add "#!/bin/sh" to the start of the file.
chmod 755 /etc/init.d/local
update-rc.d local defaults 80

# Now we can put stuff in the local file.

*******************************************************
#To set up NFS:
*******************************************************
## This installs the nfs service
sudo apt-get install nfs-kernel-server

## To check if the NFS service is running:
sudo /etc/init.d/nfs-kernel-server status

## To start the NFS service:
sudo /etc/init.d/nfs-kernel-server start


Usually the folders to be shared are kept in the /etc/exports
## Format: 
<folder path> <hostname>(permissions) 
/tmp	192.168.2.24(rw,sync,no_subtree_check)
## Share /tmp folder to the 192.168.2.24 client with read write access

## To use, mount the above shared folder on the client like following:
mkdir /tmp/mnt
sudo mount server:/tmp /tmp/mnt

## For /etc/fstab file:
ubuntu:/home/demo /nfsmount nfs

====Apt-get how to====

==Once you do changes to the /etc/apt/sources.list==
apt-get update

==This command upgrades all installed packages. This is the equivalent of "Mark all upgrades" in Synaptic.==
apt-get upgrade

==This command upgrades the entire system to a newer release==
apt-get dist-upgrade

==Reconfigure the named package==
dpkg-reconfigure fontconfig-config

==This command shows the description of package ==
apt-cache show <package_name>h

==This command provides a listing of every package in the system==
apt-cache pkgnames

====Fdisk how to====
==List current filesystems on the comp / helpful in finding the usb drive==
fdisk -l

==Compile software==
Info from : https://help.ubuntu.com/community/CompilingEasyHowTo
===Software required===
#You need to install the package build-essential for making the package and checkinstall for putting it into your package manager. 
	apt-get install build-essential checkinstall
  
#And since you may want to get code from some projects with no released version, you should install appropriate version management software. 
  apt-get install cvs subversion git-core mercurial
  
#You should then build a common directory for yourself where you'll be building these packages
  mkdir /usr/local/src
  
# The user that runs the system needs to be able to read write execute this directory
  chmod u+rwx /usr/local/src

All of your tar balls should be extracted in the above directory.
