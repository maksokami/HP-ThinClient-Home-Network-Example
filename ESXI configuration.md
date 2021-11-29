# ESXI configuration

## ESXI network configuration
In this example I'm using the realtek NIC (vmnic4) as a management interface, and the server 4-port NIC for main connectivity.  


| Port Groups | vSwitch | NIC |
| ------------- | ------------- | ------------- |
| pgWan  | vSwWAN  | vmnic0  | 
| pgPC  | vSwPC  | vmnic1  | 
| pgTrunk1  | vSwTrunk1  | vmnic2  | 
| pgVlan4Plex  | vSwTrunk2  | vmnic3  | 
| pgMgmt,VM Network  | vSwitch0  | vmnic4  |

**Change ESXI management interface IP**
pgMgmt > vSwitch0 > vmk0 > vmnic4: 192.168.1.2/24

Most of the configuration can be easily done via ESXI web GUI, however I've found it easier to change default gateway through CLI. So, to change the default gateway, enable ssh server on ESXI, login to the CLI and run the following commands:
```
esxcfg-route 192.168.1.1
esxcfg-route
```

**Trunk configuration on ESXI**
> Our OpnSense firewall needs to be a gateway/router for all vlans, therefore it needs to receive all of them on the ESXI trunk interface facing the VM. Remember to use VLAN 4095 on ESXI trunk port group that should pass all vlans. This number means "all vlans allowed".

Port Group pgTrunk1:
 - VLAN 4095 (All)
 - Security: Promiscuous mode

> If you choose to run a plex server, put the port group for that VM in the vlan4, which is allocated for this purpose. 
> If another VM must be assigned to one of the VLANs, vSwTrunk2 can be used to create a port group similarly to pgVlan4Plex
Port Group pgVL50Plex:
 - VLAN 4


## VM sizing
**OpnSense**  
Open BSD 12; RAM 3G; HDD 14G  

**Debian LTS for Plex Server**  
RAM 6G; HDD 10G; Full disk LVM, resize SWAP to 2G  

## Assigning ESXI port groups to VMs
VM Plex (VLAN4):
 - pgVlan4Plex
VM OpnSense (TRUNK):
 - pgWan
 - pgPC
 - pgTrunk1
