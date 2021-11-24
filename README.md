# HP-ThinClient-Home-Network-Example
If you ever looked at UniFi products or similar and had doubts about price and security, this repo will show how to setup a reasonably priced and secure local network with firewall, VPN, DNS filtering, VLAN support, Plex server, etc and without relying on 3rd party cloud providers.

## Parts and budget
(Prices as of 2020)
- Used cisco switch with PoE for WiFi AP (WAP) and enough port (Example: Cisco SG200-10FP, SG200-8P) ~$50-$80
- Old autonomous Cisco AP (Example: Cisco Aironet AIR-AP1142N-A-K9) ~$25
- HP Thin client, 16G Ram (Example: HP T730 Thin Client with 16GB Ram) ~$200
- 4Port Gig server NIC for the Thin client (Example: HP NC365TPCI-E) ~$25
- Ethernet Cables ~$10
- (optional)External USB drive for Plex server video storage ~$40

## Software choice
- Lvl1 hypervisor on Thin Client: VmWare ESXI. Other options to look at: KVM and Proxmox VE, XEN, Hyper-V
Althoug ESXI doesn't support Realtek NIC in the HP Thin cliet, it's fairly easy to add the driver in the older ESXI version:  
[How to: ESXI with Realtek driver](https://github.com/maksokami/HP-ThinClient-Home-Network-Example/blob/main/Build%20ESXI%20image%20with%20Realtek%20Driver.md)
- Firewall: OpnSense. Alternatives: PfSense
- Plex server: Ubuntu LTS
- (optional) External DNS filter on rasbPi (PiHole) Alternative: NxFilter

## Physical diagram
![Physical diagram](https://github.com/maksokami/HP-ThinClient-Home-Network-Example/raw/main/Home%20Network%20Diagram-Overview.png)
![Logical diagram](https://github.com/maksokami/HP-ThinClient-Home-Network-Example/raw/main/Home%20Network%20Diagram-Logical%20-%20Thin%20Client.png)

