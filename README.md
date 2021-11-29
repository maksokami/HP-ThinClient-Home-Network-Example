# HP-ThinClient-Home-Network-Example
If you ever looked at UniFi products or similar and had doubts about price and security, this repo will show how to setup a reasonably priced and secure local network with firewall, VPN, DNS filtering, VLAN support, Plex server, etc and without relying on 3rd party cloud providers.


## Requirements / Desired network
- Free firewall with rich functionality as the central router for all LAN hosts.
- Home network with multiple VLANs, each of which has a separate set of rules and restrictions.
- Home Wi-Fi with multiple SSIDs that map to multiple VLANs
- (optional) Ability to enable a DNS filter and manage it separately from the firewall with nice interface - Pi Hole on RaspPi on the diagram
- (optional) Ability to connect a stationary PC directly to the firewall with dedicated set of rules

Possible physical diagram example:  
![Physical diagram](https://github.com/maksokami/HP-ThinClient-Home-Network-Example/raw/main/Home%20Network%20Diagram-Overview.png)

## Parts and budget
(Prices as of 2020)
- Used cisco switch with PoE for WiFi AP (WAP) and enough port (Example: Cisco SG200-10FP, SG200-8P) ~$50-$80
- Old autonomous Cisco AP (Example: Cisco Aironet AIR-AP1142N-A-K9) ~$25
- HP Thin client, 16G Ram (Example: HP T730 Thin Client with 16GB Ram) ~$200
- 4Port Gig server NIC for the Thin client (Example: HP NC365TPCI-E) ~$25
- Ethernet Cables ~$10
- (optional)External USB drive for Plex server video storage ~$40

## Software choice
- Lvl1 hypervisor on Thin Client: VmWare ESXI. Other options to look at: KVM and Proxmox VE, XEN, Hyper-V.  
Although ESXI doesn't support Realtek NIC in the HP Thin cliet, it's fairly easy to add the driver in the older ESXI version:  

- Firewall: OpnSense. Alternatives: PfSense
- Plex server: Ubuntu LTS
- (optional) External DNS filter on rasbPi (PiHole) Alternative: NxFilter


## Activities
- [Upgrade BIOS on the Thin Client](https://github.com/maksokami/HP-ThinClient-Home-Network-Example/blob/main/Thin%20Client%20BIOS%20upgrade)
- [Make an ESXI image with Realtek NIC support](https://github.com/maksokami/HP-ThinClient-Home-Network-Example/blob/main/Build%20ESXI%20image%20with%20Realtek%20Driver.md)
- [Install ESXI on the Thin Client](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.esxi.install.doc/GUID-6FFA928F-7F7D-4B1A-B05C-777279233A77.html)
- [Configure the switch](https://github.com/maksokami/HP-ThinClient-Home-Network-Example/blob/main/Switch%20configuration.md)
- [Configure ESXI](https://github.com/maksokami/HP-ThinClient-Home-Network-Example/tree/main)
- [Install OpnSense](https://docs.opnsense.org/manual/install.html). Also see [Youtube: VMware ESXi 6.5 VM Creation and Guest OS installation](https://youtu.be/cWDU2I2RxAI?t=121)
- [Configure OpnSense Firewall VM](https://github.com/maksokami/HP-ThinClient-Home-Network-Example/blob/main/Firewall%20configuration.md)
- (Optional) Configure Plex server VM
- [Configure Wi-Fi Access Point](https://github.com/maksokami/HP-ThinClient-Home-Network-Example/blob/main/Home%20WAP%20configuration.md)
- (Optional) Start PiHole on the raspPi
- Connect all devices
