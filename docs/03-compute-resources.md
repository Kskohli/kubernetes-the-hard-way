# Provisioning Compute Resources

Kubernetes requires a set of machines to host the Kubernetes control plane and the worker nodes where containers are ultimately run. In this lab you will provision the compute resources required for running a secure Kubernetes cluster on VMware Workstation.

Created 3 VMs for this simple deployment.
</br> VM1 named master001, CPU-2, MEMORY-2GB, HDD-20GB, NETWORK-NAT
</br> VM2 named worker001, CPU-1, MEMORY-1GB, HDD-20GB, NETWORK-NAT
</br> VM3 named worker002, CPU-1, MEMORY-1GB, HDD-20GB, NETWORK-NAT

I choose NAT(Network Address Translation) so that I can use Internet on Ubuntu Virtual Machines as well as I connect to them using SSH from the same machine where workstation is installed.


## VM Operating System Requirements

[Download](http://releases.ubuntu.com/16.04/ubuntu-16.04.6-server-amd64.iso) the ISO for Ubuntu 16.04.6 LTS.
</br>Create it with above mentioned configuration & Install the OS with following settings:-
* You need to disable any updates from CD as we will be using internet to update & upgrade the packages.
```
nano /etc/apt/sources.list  
```
and comment(#) on line number 1. Where cdrom is mentioned, to be done on all VMs.

* It needs SWAP to be disabled for which is better explained by [@FrankDenneman in his post](https://frankdenneman.nl/2018/11/15/kubernetes-swap-and-the-vmware-balloon-driver/).
You can check the status of Swap File/Partition in OS by running **free -h** command and disable post that.

```
swapoff -a

vim /etc/fstab  
```
and comment(#) on line number 10, where swapfile/swap partition/swap word is mentioned. To be performed on all VMs.

============================================================================================================================
### Tasks to be performed on VM1:- master001

* **Static IP,Set Hostname & add /etc/hosts file with Hostname & IP** it could be done by using following:-

----------------------------------------------------------------------------------------------------------------------------
**STATIC IP Configuration**
```
sudo vim /etc/network/interfaces
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
auto ens33
iface ens33 inet static
        address 182.20.10.200
        netmask 255.255.255.0
        network 182.20.10.0
        broadcast 182.20.10.255
        gateway 182.20.10.2
        # dns-* options are implemented by the resolvconf package, if installed
        dns-nameservers 8.8.8.8
```

As When we use NAT option for network adapter, it uses an internal default gateway which is first IP i.e 182.20.10.1 and External default gateway which is configured as a Static IP on VMnet Adapter in Windows(Workstation Machine).

Post this restart Network service 


```
/etc/init.d/networking restart 

```
----------------------------------------------------------------------------------------------------------------------------
**Hostname in Ubuntu**
```
hostnamectl set-hostname master001

```
----------------------------------------------------------------------------------------------------------------------------
**Static Lookup in Tables**
```
vim /etc/hosts

```
and add following lines below first line.

```
182.20.10.200   master001
182.20.10.201   worker001
182.20.10.202   worker002

```
----------------------------------------------------------------------------------------------------------------------------

### Tasks to be performed on VM2:- worker001


* **Static IP,Set Hostname & add /etc/hosts file with Hostname & IP** it could be done by using following:-

----------------------------------------------------------------------------------------------------------------------------
**STATIC IP Configuration**
```
sudo vim /etc/network/interfaces
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
auto ens33
iface ens33 inet static
        address 182.20.10.201
        netmask 255.255.255.0
        network 182.20.10.0
        broadcast 182.20.10.255
        gateway 182.20.10.2
        # dns-* options are implemented by the resolvconf package, if installed
        dns-nameservers 8.8.8.8
```

As When we use NAT option for network adapter, it uses an internal default gateway which is first IP i.e 182.20.10.1 and External default gateway which is configured as a Static IP on VMnet Adapter in Windows(Workstation Machine).

Post this restart Network service 


```
/etc/init.d/networking restart 

```
----------------------------------------------------------------------------------------------------------------------------
**Hostname in Ubuntu**
```
hostnamectl set-hostname worker001

```
----------------------------------------------------------------------------------------------------------------------------
**Static Lookup in Tables**
```
vim /etc/hosts

```
and add following lines below first line.

```
182.20.10.201   worker001
182.20.10.200   master001
182.20.10.202   worker002

```
----------------------------------------------------------------------------------------------------------------------------


### Tasks to be performed on VM3:- worker002

* **Static IP,Set Hostname & add /etc/hosts file with Hostname & IP** it could be done by using following:-

----------------------------------------------------------------------------------------------------------------------------
**STATIC IP Configuration**
```
sudo vim /etc/network/interfaces
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
auto ens33
iface ens33 inet static
        address 182.20.10.202
        netmask 255.255.255.0
        network 182.20.10.0
        broadcast 182.20.10.255
        gateway 182.20.10.2
        # dns-* options are implemented by the resolvconf package, if installed
        dns-nameservers 8.8.8.8
```

As When we use NAT option for network adapter, it uses an internal default gateway which is first IP i.e 182.20.10.1 and External default gateway which is configured as a Static IP on VMnet Adapter in Windows(Workstation Machine).

Post this restart Network service 


```
/etc/init.d/networking restart 

```
----------------------------------------------------------------------------------------------------------------------------
**Hostname in Ubuntu**
```
hostnamectl set-hostname worker002

```
----------------------------------------------------------------------------------------------------------------------------
**Static Lookup in Tables**
```
vim /etc/hosts

```
and add following lines below first line.

```
182.20.10.202   worker002
182.20.10.200   master001
182.20.10.201   worker001

```
----------------------------------------------------------------------------------------------------------------------------


============================================================================================================================

Now **Update & Upgrade**  the packages of all the VMs i.e master001, worker001, worker002 by running the following

```
sudo apt update -y

Sudo apt upgrade -y
```

### Install openssh server in all the VMs i.e master001, worker001, worker002

```
sudo apt install openssh-server -y
```


### Optional, Enable root user in Ubuntu & Set password.

```
vim /etc/ssh/sshd_config 

Permitrootlogin yes 

 

Set password for root user  

passwd root 

 
```



Next: [Provisioning a CA and Generating TLS Certificates](04-certificate-authority.md)
