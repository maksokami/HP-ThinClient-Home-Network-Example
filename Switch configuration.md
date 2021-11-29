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

