### System-On-Module (SOM)
A **System-On-Module (SOM)** is as small circuit board that carries the brain of an embedded system (main processor, memory, and power) as a single pluggable unit. Rather than creating all of these from scratch, a system simply connects a SOM to a large carrier board with connectors and I/O. 

The **Kria K26 SOM** is AMD/Xilinx's module; the **KV260 Vision AI Starter Kit** is the carrier board designed around it, designed for camera/vision use cases.
### Zynq UltraScale+ MPSoC
The core processor of the K26 SOM is the *Zynq UltraScale+ MPSoC*. It is **heterogenous**, meaning it contains different kinds of compute on one chip:
- **APU**: contains four ARM Cortex-A53 64-bit cores running Linux and perform general purpose work. Everything typed in the Kria shell runs here.
- **RPU**: Two ARM Cortex-R5F cores for bare-metal, deterministic real-time firmware.
- **PL**: [[Programming Logic (PL)||The FPGA fabric]]. Custom digital hardware is defined here, like the lab's NAND gate.
- **PMU (Platform Management Unit) with CSU (Configuration Security Unit)**: Dedicated controllers that manage power, boot, and configuration of everything above.

The APU (software) and PL (custom hardware) talk to each other over an on-chip bus standard called **AXI**. Below is the KV260 at a glance.
![[Pasted image 20260928120307.png]]
### PetaLinux
**PetaLinux** is AMD/Xilinx's toolset for building an embedded Linux distribution tailored to a Zynq device. It was built into the Kria. There are two artifacts that matter conceptually:
- **BOOT.BIN**: the first thing the chip loads at power-on. Contains small startup programs (bootloaders) + PMU firmware (helper processor that manages power).
	- The bootloader finds the OS and loads it into memory.
- **image.ub**: a bundle holding the Linux kernel, the base device tree (tree of all USB, memory, networking chip, etc.), and the root filesystem.

When the device is powered on, the PMU+CSU loads BOOT.BIN, which brings up the bootloader, which loads image.ub, which boots Linux on the A53 cores until you reach the login prompt. The custom PL accelerator is not part of this base image (on boot, it does not exist); it is layered on at *runtime*, which is what makes xmutil and device-tree overlays necessary. Rebuilding the whole image for every accelerator change is slow and complex. Layering at runtime allows us to swap out accelerators quickly while not touching the base system.
### AXI and AXI GPIO
**Advanced eXtensible Interface (AXI)** is a bus protocol used by ARM cores to read/write to registers inside PL as if they were located at addresses in memory. An **AXI GPIO** block is a small PL peripheral that exposes GPIO pins to software/CPU. In this lab, there are two AXI GPIO blocks; one drives its two inputs, the other reads its output.
### Device Tree
Linux does not auto-detect memory-mapped hardware the way it detects a USB stick. Instead, it uses a **device tree**, which is a text description (compiled to a binary .dtb) listing all hardware, along with their addresses and what drivers to bind. This is the only way to reveal the NAND accelerator to Linux.
#### Device-Tree Overlay (.dtbo)
We cannot edit the base device tree after boot. A **device-tree overlay** is a small patch applied on top of the base device tree to announce new hardware. 

When you load the accelerator, xmutil programs the FPGA with your bitstream (design file) and applies your .dtbo. Linux discovers the two AXI GPIO blocks, binds the gpio-xilinx driver to them, and exposes them to user space under /sys/class/gpio.
### xmutil and XRT
**XRT (Xilinx Runtime)** is the software layer managing what is loaded into the PL, while **xmutil** is the command-line front end. The following commands will be used:
- xmutil listapps: show registered accelerators and which slot is active.
- xmutil loadapp \<name>: program the PL + apply overlay.
- xmutil unloadapp: (remove accelerator).
### GPIO sysfs vs. Memory-Mapped I/O
To communicate with GPIO, there is a software solution and hardware solution.

**GPIO sysfs** is a high-level, user-friendly software solution. Linux has a gpio-xilinx driver that knows how to talk to AXI GPIO blocks. It presents each pin as a file under /sys/class/gpio. You set or read a pin by writing/reading that file. It is simple but slow, as every access is a filesystem operation:
1. The program asks the kernel to write/read a file.
2. Kernel switches from user to kernel mode (relatively expensive).
3. The kernel finds the file, checks permissions, and hands the request to the GPIO driver.
4. The driver touches the actual hardware register.
5. Control returns back to the program.

With **Memory-Mapped I/O (MMIO)**, the C++ program does a one-time setup:
1. Opens /dev/mem, a special file representing the machine's physical memory.
2. Calls **mmap()**, which maps the AXI GPIO registers straight into the program's address space.
After this, the register appears in the program as an ordinary pointer.