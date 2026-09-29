The base device tree lists the addresses of all devices at boot time. To add a new device, it is not feasible to rebuild it and reboot every time. For this, Linux supports a **device-tree overlay**, a patch applied to the live tree while the system still runs. 

The patch is compiled into a binary with dtc (**device-tree compiler**), producing a .dtbo (**device-tree blob, overlay**).