---
title: 'Home Server - Part 2 - pfSense and piHole Install'
date: 2022-07-05 15:01:01 +/-0000
categories: [HomeServer, Hardware]
tags: [hardware, server, homelab, ryzen, pfsense, proxmox]
---

## pfSense setup
```
Download pfsense iso
click local storage -> iso images -> upload
Choose pfsense iso
Create VM (top right)
Give a name (pfsense), click next
choose pfsense image from iso image dropdown
choose linux 5.x
click next
click next
click next
Increase cores to 4
click next
Reduce RAM to 1024MB
click next
uncheck firewall
click next
click finish

click the pfsense vm in the left panel
click hardware
add network device
choose the second bridge device from the dropdown
click add
take note of the mac address of the second network adapter

click Start
follow the installation of pfsense
choose no when asked to set up vlans
choose the adapter with the mac address we noted down when asked about the WAN interface
choose the other adapter when asked about the LAN interface

Disconnect the server from your current LAN Setup and directly attach to your main machine until configuration is complete.

```
## piHole - dns adblocker and local dns server