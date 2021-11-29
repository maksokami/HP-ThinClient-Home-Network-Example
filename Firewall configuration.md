# Firewall configuration (OpnSense)

## Interfaces and IP addresses
> In our example we have a stationary PC which needs to be connected directly to the firewall for some reason (vmx2-IFPC, pgPC). This is completely optional.


| ESXi NIC | OpnSense Interface | IP | Notes |
| ------------- | ------------- | ------------- | ------------- |
| pgWan  | vmx1-WAN  | DHCP  | Internet/ISP Modem connection |
| pgPC  | vmx2-IFPC  | 192.168.5.1/24  | Direct PC connection |
| pgLAN | vmx3-IFLAN  | 192.168.1.1/24  | Management, native vlan on Trunk | 
| pgLAN | vmx3-vl2-IF2USR  | 192.168.2.1/24 | Users (Computers and phones) | 
| pgLAN | vmx3-vl3-IF3GST | 192.168.3.1/24 | Guests (Internet only vlan) | 
| pgLAN | vmx3-vl4-IF4TV  | 192.168.4.1/24 | TV and Plex | 

## DHCP server
| Interface | DHCP range | DNS |
------ | ------------- | ------------- |
|  vmx2-IFPC  | 192.168.5.100-110/24 | Default |
| vmx3-vl2-IF2USR | 192.168.2.100-150 | PiHole | 
| vmx3-vl3-IF3GST | 192.168.3.100-150 | PiHole | 
| vmx3-vl4-IF4TV | 192.168.4.100-150 | Default or custom rules (TVs are known to have issues with DNS filters) | 


## Firewall rules
. . .   


## Other services to consider
- Intrusion Detection
  - Enable on WAN
- DynamicDNS
- WireGuard/OpenVPN for remote access

## Plugins to consider
Ntop monitoring: https://www.ntop.org/guides/ntopng/third_party_integrations/opnsense.html

