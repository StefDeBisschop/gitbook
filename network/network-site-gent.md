# Network Site Gent

The network at the site in Gent is the most complex. I will be explaining this step-by-step where I will cover most of the technologies used.

<figure><img src="../.gitbook/assets/Network_Gent.png" alt=""><figcaption><p>Network Site Gent</p></figcaption></figure>

### VLAN's

I use VLAN to seperate the different networks from each other. It's just the same as on the server and used on the firewall to make rules.

```
SR-Switch01#show vlan

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/7, Fa0/8, Fa0/9, Fa0/12
                                                Fa0/13, Fa0/14, Fa0/15, Fa0/16
                                                Fa0/17, Fa0/18, Fa0/19, Fa0/22
                                                Fa0/23, Gig0/2
10   Gent-Servers                     active    Fa0/1, Fa0/2
15   Gent-Clients                     active    Fa0/6
16   Gent-DMZ-Servers                 active    Fa0/3, Fa0/4, Fa0/5
160  Gent-Wireless-Clients            active    
```

### VTP

VTP stands for VLAN Trunking Protocol, a cisco protocol that makes it possible to advertise the VLAN's to all switches in that domain (VTP domain). On this network, the L3 switch is the VTP server with pandoraVTP as domain. The other switches in the network are both clients on that domain.

{% code title="The VTP server status" %}
```
SR-Switch01#show vtp status 
VTP Version capable             : 1 to 2
VTP version running             : 1
VTP Domain Name                 : pandoraVTP
VTP Pruning Mode                : Disabled
VTP Traps Generation            : Disabled
Device ID                       : 0001.43BA.1000
Configuration last modified by 0.0.0.0 at 3-1-93 00:00:00
Local updater ID is 192.168.0.1 on interface Vl1 (lowest numbered VLAN interface found)

Feature VLAN : 
--------------
VTP Operating Mode                : Server
Maximum VLANs supported locally   : 1005
Number of existing VLANs          : 9
Configuration Revision            : 91
MD5 digest                        : 0x6E 0x87 0x88 0x7F 0x71 0xD1 0x92 0x75 
                                    0x37 0x9C 0xCF 0x7B 0xE1 0xEC 0xCE 0x60 
```
{% endcode %}

