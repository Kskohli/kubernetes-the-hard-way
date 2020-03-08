# Prerequisites


## VM Hardware Requirements
Created 3 VMs for this simple deployment.
</br> VM1 named master001, CPU-2, MEMORY-2GB, HDD-20GB, NETWORK-NAT
</br> VM2 named worker001  CPU-2, MEMORY-2GB, HDD-20GB, NETWORK-NAT
</br> VM3 named worker002  CPU-2, MEMORY-2GB, HDD-20GB, NETWORK-NAT

I choose NAT(Network Address Translation) so that I can use Internet on Ubuntu Virtual Machines as well as I connect to them using SSH from the same machine where workstation is installed.

## VM Operating System Requirements
[Download](http://releases.ubuntu.com/16.04/ubuntu-16.04.6-server-amd64.iso) the ISO for Ubuntu 16.04.6 LTS.
</br>Create it with above mentioned configuration & Install the OS with following settings:-
* You need to disable any updates from CD as we will be using internet to update & upgrade the packages.

```
nano /etc/apt/sources.list  
```
and comment(#) on line number 1. Where cdrom is mentioned.

* It needs SWAP to be disabled for which is better explained by [@FrankDenneman in his post](https://frankdenneman.nl/2018/11/15/kubernetes-swap-and-the-vmware-balloon-driver/).
You can check the status of Swap File/Partition in OS by running **free -h** command and disable post that.

```
swapoff -a

vim /etc/fstab  
```
and comment(#) on line number 10, where swapfile/swap partition/swap word is mentioned. 


*
*
*


## VMware Workstation or Fusion for mac

You need to have some basic knowledge of networking in VMware Workstation using [Network Editor ](https://pubs.vmware.com/workstation-11/index.jsp?topic=%2Fcom.vmware.ws.using.doc%2FGUID-D9B0A52D-38A2-45D7-A9EB-987ACE77F93C.html).
You might need a License for VMware Workstation or Fusion for mac. 




### Install the Google Cloud SDK

Follow the Google Cloud SDK [documentation](https://cloud.google.com/sdk/) to install and configure the `gcloud` command line utility.

Verify the Google Cloud SDK version is 262.0.0 or higher:

```
gcloud version
```

### Set a Default Compute Region and Zone

This tutorial assumes a default compute region and zone have been configured.

If you are using the `gcloud` command-line tool for the first time `init` is the easiest way to do this:

```
gcloud init
```

Then be sure to authorize gcloud to access the Cloud Platform with your Google user credentials:

```
gcloud auth login
```

Next set a default compute region and compute zone:

```
gcloud config set compute/region us-west1
```

Set a default compute zone:

```
gcloud config set compute/zone us-west1-c
```

> Use the `gcloud compute zones list` command to view additional regions and zones.

## Running Commands in Parallel with tmux

[tmux](https://github.com/tmux/tmux/wiki) can be used to run commands on multiple compute instances at the same time. Labs in this tutorial may require running the same commands across multiple compute instances, in those cases consider using tmux and splitting a window into multiple panes with synchronize-panes enabled to speed up the provisioning process.

> The use of tmux is optional and not required to complete this tutorial.

![tmux screenshot](images/tmux-screenshot.png)

> Enable synchronize-panes by pressing `ctrl+b` followed by `shift+:`. Next type `set synchronize-panes on` at the prompt. To disable synchronization: `set synchronize-panes off`.

Next: [Installing the Client Tools](02-client-tools.md)
