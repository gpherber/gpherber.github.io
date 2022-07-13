---
title: 'Home Server - GPU Passthrough to Proxmox VM'
date: 2022-07-05 15:01:01 +/-0000
categories: [HomeServer, Hardware]
tags: [hardware, server, homelab, ryzen, pfsense, proxmox]
---

## GPU Passthrough - Proxmox 7.2

### BIOS/UEFI options
```
Enable IOMMU / VT-d in BIOS options
```

### Edit /etc/default/grub/

Change intel_iommu for amd_iommu on amd systems

```
GRUB_CMDLINE_LINUX_DEFAULT="textonly quiet intel_iommu=on video=vesafb:off video=efifb:off video=simplefb:off"
```
### Update GRUB
``` bash
update-grub
```
### Edit /etc/modules/
```
vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd
```
### Edit /etc/modprobe.d/vfio.conf
Change id's to match your graphics card.

```
options vfio-pci ids=10de:1430,10de:0fba disable_vga=1
```
### Update module blacklist
``` bash
echo "blacklist radeon" >> /etc/modprobe.d/blacklist.conf
echo "blacklist nouveau" >> /etc/modprobe.d/blacklist.conf
echo "blacklist nvidia" >> /etc/modprobe.d/blacklist.conf
echo "options kvm ignore_msrs=1" > /etc/modprobe.d/kvm.conf
update-initramfs -u
```
### create /root/fix_gpu_pass.sh
``` bash
cd /root/
touch fix_gpu_pass.sh
chmod +x fix_gpu_pass.sh
nano fix_gpu_pass.sh
```
replace pci device with correct number from lspci
``` bash
#!/bin/bash
echo 1 > /sys/bus/pci/devices/0000\:04\:00.0/remove
echo 1 > /sys/bus/pci/rescan
```
### Edit crontab
``` bash
crontab -e
```
```
@reboot /root/fix_gpu_pass.sh
```
Reboot the system

Passthrough gpu using web ui