# Home WAP configuration

## Cisco  autonomous AP configuration. Example with trunk uplink and multiple SSIDs

**Secrets**:  
Pay attention that there are multiple places where passwords are specified. 
- username - Credentials for CLI access
- enable secret - Will be used to transition from user exec mode to privileged exec mode during CLI access.
- wpa-psk - Wi-Fi network password for the selected SSID
  
This configuration is dual-band.  
2.4Ghz radio is configured to switch between channels 1,6,11, as per best practices.    
5GHz radio is configured to use one selected channel (scan your house and see which one would you prefer).  
Wi-Fi channel list can be found [here](https://en.wikipedia.org/wiki/List_of_WLAN_channels)

### Configuration
Establish console access to the AP. Erase existing config and reboot:
```
! Erase current config
wr erase
reboot
!
```
Enter configuration mode:
```
hostname>
hostname> enable
hostname# configure
hostname(config)#
```
Copy-paste the configuration:
```
!
no service pad
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
!
hostname my-home-ap
!
enable secret 5 XXXXXXX
!
no aaa new-model
! Add, if home DNS will be used:
!ip domain name .x.com
!
!
dot11 syslog
!
dot11 ssid Users
   vlan 2
   authentication open 
   authentication key-management wpa version 2
   mbssid guest-mode
   wpa-psk ascii 7 XXXXXXX
!
dot11 ssid Guests
   vlan 3
   authentication open 
   authentication key-management wpa version 2
   mbssid guest-mode
   wpa-psk ascii 7 XXXXXXX
!
!
username admin password 7 XXXXXXX
!
!
bridge irb
!
!
interface Dot11Radio0
 description WIRELESS | 2.4Ghz
 no ip address
 no ip route-cache
 !
 encryption vlan 3 mode ciphers aes-ccm 
 !
 encryption vlan 2 mode ciphers aes-ccm 
 !
 encryption vlan 1 mode ciphers aes-ccm 
 !
 ssid Users
 !
 ssid Guests
 !
 antenna gain 0
 mbssid
 channel least-congested 2412 2437 2462
 station-role root
!
interface Dot11Radio0.1
 description Legacy
 encapsulation dot1Q 1 native
 no ip route-cache
 bridge-group 1
 bridge-group 1 subscriber-loop-control
 bridge-group 1 block-unknown-source
 no bridge-group 1 source-learning
 no bridge-group 1 unicast-flooding
 bridge-group 1 spanning-disabled
!
interface Dot11Radio0.2
 encapsulation dot1Q 2
 no ip route-cache
 bridge-group 2
 bridge-group 2 subscriber-loop-control
 bridge-group 2 block-unknown-source
 no bridge-group 2 source-learning
 no bridge-group 2 unicast-flooding
 bridge-group 2 spanning-disabled
!
interface Dot11Radio0.3
 encapsulation dot1Q 3
 no ip route-cache
 bridge-group 3
 bridge-group 3 subscriber-loop-control
 bridge-group 3 block-unknown-source
 no bridge-group 3 source-learning
 no bridge-group 3 unicast-flooding
 bridge-group 3 spanning-disabled
!
!
interface Dot11Radio1
 description WIRELESS | 5Ghz
 no ip address
 no ip route-cache
 !
 encryption vlan 2 mode ciphers aes-ccm 
 !
 encryption vlan 3 mode ciphers aes-ccm 
 !
 encryption vlan 1 mode ciphers aes-ccm 
 !
 ssid Users
 !
 ssid Guests
 !
 antenna gain 0
 dfs band 3 block
 mbssid
 channel 5785
 station-role root
!
interface Dot11Radio1.1
 encapsulation dot1Q 1 native
 no ip route-cache
 bridge-group 1
 bridge-group 1 subscriber-loop-control
 bridge-group 1 block-unknown-source
 no bridge-group 1 source-learning
 no bridge-group 1 unicast-flooding
 bridge-group 1 spanning-disabled
!
interface Dot11Radio1.2
 encapsulation dot1Q 2
 no ip route-cache
 bridge-group 2
 bridge-group 2 subscriber-loop-control
 bridge-group 2 block-unknown-source
 no bridge-group 2 source-learning
 no bridge-group 2 unicast-flooding
 bridge-group 2 spanning-disabled
!
interface Dot11Radio1.3
 encapsulation dot1Q 3
 no ip route-cache
 bridge-group 3
 bridge-group 3 subscriber-loop-control
 bridge-group 3 block-unknown-source
 no bridge-group 3 source-learning
 no bridge-group 3 unicast-flooding
 bridge-group 3 spanning-disabled
!
interface GigabitEthernet0
 description INTERCONNECT | To Switch
 no ip address
 no ip route-cache
 duplex auto
 speed auto
 no keepalive
!
interface GigabitEthernet0.1
 description INTERCONNECT | MGMT(Native Vlan)
 encapsulation dot1Q 1 native
 no ip route-cache
 bridge-group 1
 no bridge-group 1 source-learning
 bridge-group 1 spanning-disabled
!
interface GigabitEthernet0.2
 encapsulation dot1Q 2
 no ip route-cache
 bridge-group 2
 no bridge-group 2 source-learning
 bridge-group 2 spanning-disabled
!
interface GigabitEthernet0.3
 encapsulation dot1Q 3
 no ip route-cache
 bridge-group 3
 no bridge-group 3 source-learning
 bridge-group 3 spanning-disabled
!
interface BVI1
 description VIRTUAL | Management IP
 ip address 192.168.1.S 255.255.255.0
 no ip redirects
 no ip proxy-arp
 no ip route-cache
!
ip default-gateway 192.168.1.1
! Disable http server on the AP, if it' not required:
! no ip http server
no ip http secure-server
bridge 1 route ip
!
!
!
line con 0
 privilege level 15
line vty 0 5
 privilege level 15
 login local
 transport input ssh
!
```
Save configuration:
```
! To exit configuration mode:
end
! To save configuration
wr
! or copy run start
```
