---
description: Deploying Client Devices
---

# Auto Deployment

For the Auto Deployment of Windows devices I decided to go with FOG Project. It's 'A free open-source network computer cloning and management solution**'.** This allows us to deploy and manage Windows distributions. FOG doesn't use any boot disks, or CDs; everything is done via TFTP and PXE. The PCs boot via PXE and automatically downloads a small linux client doing all the work of imaging the machine.

FOG is best implemented on a dedicated server. It runs on a few Linux distro's, but we choose Ubuntu 22.04 for consistency accross our infrastructure.

## Setting up the VM

The VM for running a Ubuntu 22.04 server that hosts FOG has following hardware;

* 1 CPU
* 4 GB RAM
* 70GB HARD DISK

The disk space needs to be sufficient for storing the image(s) that we'll be capturing from a Golden device later.&#x20;

The FOG installer comes as a complex shell script that will handle all the package installs and configuring the services for us. Running the installer on a the system for the first time will ask us a couple of questions regarding our network configuration and services we want to install.

```
git clone https://github.com/fogproject/fogproject.git fogproject-master
```

Now we can run the installer.

```
./installfog.sh
```

```vim
FOG Server installation modes:
      * Normal Server: (Choice N) 
          This is the typical installation type and
          will install all FOG components for you on this
          machine.  Pick this option if you are unsure what to pick.

      * Storage Node: (Choice S)
          This install mode will only install the software required
          to make this server act as a node in a storage group
```

Followup questions include:

* What is the IP address to be used by this FOG Server?
* Would you like to setup a router address for the DHCP Server? \[Y/n]
* Would you like DHCP to handle DNS?
* What DNS address should DHCP allow?
* Would you like to use the FOG Server for DHCP Service? \[y/N]
* This version of FOG has internationalization support, would you like to install the additional language packs? \[y/N]
* Are you sure you wish to continue (Y/N)

After completing all of the setup questions, the installation will start. When done we are able to connect to the server via HTTP in a web browser.

