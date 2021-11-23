# Build ESXI image with Realtek 8168/8111/8411/8118 Driver
## Required tools

- Windows 7, 8, 10 and PowerShell

- Download ESXI Customizer script https://github.com/VFrontDe/ESXi-Customizer-PS
  - Documentation: https://www.v-front.de/p/esxi-customizer-ps.html
  
- Install VMWare PowerCLI https://code.vmware.com/web/tool/12.1.0/vmware-powercli
  - Prepare Nugit (Disable TLS <= 1.2) https://devblogs.microsoft.com/nuget/deprecating-tls-1-0-and-1-1-on-nuget-org/
  - https://www.cyrill-gremaud.ch/force-powershell-to-use-tls-1-2/
  - Download latest Nugit to systems32 folder: https://www.nuget.org/downloads

- Realtek driver for ESXI https://vibsdepot.v-front.de/wiki/index.php/Net55-r8168

- Rufus https://rufus.ie/en_US/

## Build iso

```
Downloads> .\ESXi-Customizer-PS.ps1 -v67 -ozip
```
- `v67` flag gets us version 6.7
- `ozip` flag gets us an offline bundle rather than the ISO.

```
Add-EsxSoftwareDepot ".\net55-r8168-8.045a-napi-offline_bundle.zip", "ESXi-6.7.0-20210304001-standard.zip" 

Get-EsxImageProfile
New-EsxImageProfile -CloneProfile ESXi-6.7.0-20210304001-standard  -name ESXi-6.7.0-20210304001-standard-RTL8111 -Vendor Razz
Set-EsxImageProfile ESXi-6.7.0-20210304001-standard-RTL8111 -AcceptanceLevel CommunitySupported

Get-EsxSoftwarePackage
Add-EsxSoftwarePackage -ImageProfile ESXi-6.7.0-20210304001-standard-RTL8111 -SoftwarePackage net55-r8168

Export-EsxImageProfile -ImageProfile ESXi-6.7.0-20210304001-standard-RTL8111 -ExportToIso -filepath .\VMware-ESXi-6.7.0-20210304001-RTL8111.iso
```

Use rufus to prepare a bootable USB stick and install ESXI. 

## Sources
- https://www.sysadminstories.com/2018/08/adding-realtek-8111-driver-to-vsphere.html?m=1
- https://blog.avojak.com/2020/12/19/installing-esxi-for-realtek-8111-nic/
