# Switch configuration

## Accessing management interface
Depending on which switch model you will choose, management access will be different. You may need a serial or usb console cable and putty-like terminal software. Some switches only have web interface enabled by deafult without console or ssh. In this case you will need to reset the switch to defaults and configure via direct ethernet connection.
Find configuration guide for your model. E.g. for Cisco SG200-10FP:
- [Configuration Guide](https://www.cisco.com/c/dam/en/us/td/docs/switches/lan/csbss/sf20x_sg20x/administration_guide/Cisco_200Sx_v1_4_AG.pdf)
- [Product Page](https://www.cisco.com/c/en/us/support/switches/small-business-200-series-smart-switches/series.html)

## VLAN list
- VLAN1 - 192.168.1.0/24 - Management VLAN. This vlan will be used by management interfaces of the switch, home WAP, and ESXI.
- VLAN2 - 192.168.2.0/24 - User VLAN. This vlan can be used for home users. It may have DNS filtering and parental control configured
- VLAN3 - 192.168.3.0/24 - Guest VLAN. Devices on this VLAN can access internet only and have no access to the local home network. This vlan may have DNS filtering, captive portal, and other restrictions.
- VLAN4 - 192.168.4.0/24 - TV and Plex server Vlan. Devices can talk to other devices in this vlan or to the internet, but not to other vlans.
- Any other VLANs you need


## Configuration details

In this example management VLAN is 1

| Interface | Mode | PoE | VLAN | Description
------ | ------------- | ------------- | ------------- | ------------- |
| Ge1  | Access  | Y | 2 | |
| ...  | ...  |   |   | |
| Ge5  | Access  | Y | 2 ||
| Ge6  | Access  | Y | 3 | Work WAP |
| Ge7  | Trunk  | Y | all | Home WAP |
| Ge8  | Access  | Y | 1 | RaspPi |
| Ge9  | Trunk  | N | all | Trunk to VMs |
| Ge10  | Trunk  | N | all | Trunk to OpnSense FW |

**Note on link aggregation**
> Why there is no link aggregation on the switch towards ESXI? 
> OpnSense VM needs a full trunk to manage all VLANs. If another VM like Plex server needs to run in parallel, static LAG must be terminated on ESXi. 
> This dictates the following ESXI configuration: LAG is terminated on ESXI (meaning no LACP support). ESXI LAG interface will be binded to OpnSense VM. ESXI port group set to vlan4 and based on LAG interface will be binded to the Plex server (or another VM, if needed). 
> Without VSphere license there is a know issue arp issue for this configuration, even if LAG algorithm matches with the switch. When I've attempted to do this setup I saw random arp resolution issues across all VLANs and OpnSense forum confirmed that this setup is not desirable. I've decided to not aggregate switch and OpnSense links. In a normal LAN a 1Gbps interface between a switch and a firewall shouldn't be overutilized considering normal inter-vlan traffic. Same will be true for a switch to Plex server connection, where 1Gbps is way more than will ever be needed.
