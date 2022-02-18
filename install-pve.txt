```
detailed installation on Proxmox VE7 with 5.13 Kernel.
tested with TP-Link UE306 USB Dongles - worked fine https://www.tp-link.com/us/home-networking/usb-converter/ue306/  

root@pve-1:~# apt install build-essential pve-headers-5.13 bison flex autoconf automake make build-essential

root@pve-1:~# tar -xjvf AX88179_178A_Linux_Driver_v1.20.0_source_patched_kernel.5.x.tbz2
AX88179_178A_Linux_Driver_v1.20.0_source_patched_kernel.5.x/
AX88179_178A_Linux_Driver_v1.20.0_source_patched_kernel.5.x/ax88179_178a.conf
AX88179_178A_Linux_Driver_v1.20.0_source_patched_kernel.5.x/Makefile
AX88179_178A_Linux_Driver_v1.20.0_source_patched_kernel.5.x/ax88179_178a.c
AX88179_178A_Linux_Driver_v1.20.0_source_patched_kernel.5.x/readme
AX88179_178A_Linux_Driver_v1.20.0_source_patched_kernel.5.x/ax88179_178a.h

root@pve-1:~# cd AX88179_178A_Linux_Driver_v1.20.0_source_patched_kernel.5.x/
root@pve-1:~/AX88179_178A_Linux_Driver_v1.20.0_source_patched_kernel.5.x# make
make -C /lib/modules/5.13.19-4-pve/build M=/root/AX88179_178A_Linux_Driver_v1.20.0_source_patched_kernel.5.x modules
make[1]: Entering directory '/usr/src/linux-headers-5.13.19-4-pve'
  CC [M]  /root/AX88179_178A_Linux_Driver_v1.20.0_source_patched_kernel.5.x/ax88179_178a.o
  MODPOST /root/AX88179_178A_Linux_Driver_v1.20.0_source_patched_kernel.5.x/Module.symvers
  CC [M]  /root/AX88179_178A_Linux_Driver_v1.20.0_source_patched_kernel.5.x/ax88179_178a.mod.o
  LD [M]  /root/AX88179_178A_Linux_Driver_v1.20.0_source_patched_kernel.5.x/ax88179_178a.ko
make[1]: Leaving directory '/usr/src/linux-headers-5.13.19-4-pve'
root@pve-1:~/AX88179_178A_Linux_Driver_v1.20.0_source_patched_kernel.5.x# cp ax88179_178a.ko /lib/modules/5.13.19-4-pve/kernel/drivers/net/usb/ax88179_178a.ko
cp: overwrite '/lib/modules/5.13.19-4-pve/kernel/drivers/net/usb/ax88179_178a.ko'? yes
root@pve-1:~/AX88179_178A_Linux_Driver_v1.20.0_source_patched_kernel.5.x# rmmod ax88179_178a
root@pve-1:~/AX88179_178A_Linux_Driver_v1.20.0_source_patched_kernel.5.x# modprobe ax88179_178a bEEE=0 bGETH=0
root@pve-1:~/AX88179_178A_Linux_Driver_v1.20.0_source_patched_kernel.5.x# cp ax88179_178a.conf /etc/modprobe.d/
root@pve-1:~/AX88179_178A_Linux_Driver_v1.20.0_source_patched_kernel.5.x# systemctl restart networking.service

 >> dmesg
...
[127836.622623] usbcore: deregistering interface driver ax88179_178a
[127836.623468] ax88179_178a 2-1:1.0 eth1: unregister 'ax88179_178a' usb-0000:00:14.0-1, ASIX AX88179 USB 3.0 Gigabit Ethernet
[127839.562863] ASIX USB Ethernet Adapter:v1.20.0		http://www.asix.com.tw
[127839.562867] ax88179_178a 2-1:1.0 (unnamed net_device) (uninitialized): mtu 1500
[127839.563008] ax88179_178a 2-1:1.0 eth1: register 'ax88179_178a' at usb-0000:00:14.0-1, , 7c:c2:c6:28:b3:89
[127839.563051] usbcore: registered new interface driver ax88179_178a

root@pve-1:~/AX88179_178A_Linux_Driver_v1.20.0_source_patched_kernel.5.13# modinfo ax88179_178a
filename:       /lib/modules/5.13.19-3-pve/kernel/drivers/net/usb/ax88179_178a.ko
license:        GPL
description:    ASIX AX88179_178A USB 2.0/3.0 Ethernet Devices
author:         David Hollis
srcversion:     C65AED2C60B74A92CB4E556
alias:          usb:v0711p0179d*dc*dsc*dp*ic*isc*ip*in*
alias:          usb:v2001p4A00d*dc*dsc*dp*ic*isc*ip*in*
alias:          usb:v04E8pA100d*dc*dsc*dp*ic*isc*ip*in*
alias:          usb:v0930p0A13d*dc*dsc*dp*ic*isc*ip*in*
alias:          usb:v17EFp304Bd*dc*dsc*dp*ic*isc*ip*in*
alias:          usb:v0DF6p0072d*dc*dsc*dp*ic*isc*ip*in*
alias:          usb:v0B95p178Ad*dc*dsc*dp*ic*isc*ip*in*
alias:          usb:v0B95p1790d*dc*dsc*dp*ic*isc*ip*in*
depends:        usbnet,mii
retpoline:      Y
name:           ax88179_178a
vermagic:       5.13.19-3-pve SMP mod_unload modversions
parm:           msg_enable:usbnet msg_enable (int)
parm:           bsize:RX Bulk IN Queue Size (int)
parm:           ifg:RX Bulk IN Inter Frame Gap (int)
parm:           bEEE:EEE advertisement configuration (int)
parm:           bGETH:Green ethernet configuration (int)
```
