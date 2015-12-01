# PxIexpress
PxI Express ''Open" Game Console/Set Top Box

Right now this is all speculation. AMD's Zen has recently taped out, and open source drops are occuring relating to it.

This is an open hardware project shaped around the AMD ''Summit Ridge" APU, due to it's sixteen PCI Express lanes.

![SummitRidge](https://raw.githubusercontent.com/kamilion/PxIexpress/master/images/amd_summit_ridge_features_capabilities.jpg)

A motherboard design will be developed around either a socketed AMD processor, or if unavailable, variants of the BGA processor soldered directly to the board.

The physical design is ROUGHLY as follows:
* External power brick providing 10v-15.8V unregulated power if 16v capacitors are used or >21V regulated power if 24v capacitors are used.
* This design should be automotive and airline compatible, as well as being operational from common lead acid batteries.
* Aluminium outer chassis (custom folded aluminium sheet)
* MiniITX size 170mm x 170mm (6.7 x 6.7 in) motherboard (Or smaller)
* Both sides of the board will be utilized for maximum user expansion.
* Two unpopulated DDR4 SODIMM slots for user memory expansion, requirement of AMD central processor with integrated 2GB/4GB/8GB ''HBM" memory stacks.
* The bottom shall contain no less than two SATA compatible sockets with 12V, 5V, and 3.3V power suitable for mounting existing SATA SSD and 2.5" hard disks upside down.
* Two or more standard dual port USB headers shall also be provided on the bottom of the board. It is yet to be determined if these should be 2.0 or 3.0 style headers.
* The top of the board shall contain the processor and a single top mounted female PCI Express X8 socket situated as a ''Cartridge" port would be on a classic game console.
* The right side of the board shall end in a male PCI Express edge, most likely 8 links unless a suitable inexpensive PCIE bridge is found to route 16 links.
* A U-shaped PCB containing two female PCI Express sockets will be developed to link the main board with an optional expansion chassis directly below.
* Wireless shall be provided by an Atheros ath10k driver supported MiniPCI Express adapter. It is yet to be determined which side of the board to place it on.
* Four USB type C connectors along the front edge, providing connections to controllers and other user devices such as webcams.
* Four full sized SD Card slots, corresponding with each 'controller' port. These may be provided via 45 degree header in a ''// \\" pattern to give an 'eyebrow' appearance.
* SD Port 1 shall be connected to the processor and optionally used as a trusted platform boot device, port 2, 3 and 4 may be provided by discrete SD to USB controllers.
* GPIO implimented developer switch, likely based upon removing a physical screw from the motherboard allowing for the separation of two contacts.
* At least one rear output will be MicroHDMI, to allow direct connection of an inexpensive head mounted display such as http://www.vufine.com/

The software design is ROUGHLY as follows:
* Large firmware storage device, partitioned. Potentially an eMMC device, bumping SD port 1 to a discrete SD to USB controller?
* Processor bringup by coreboot, loading blobs from a signed firmware image over 16MB in size. (Nonfree blobs here still?)
* Hypervisor should be resident in firmware, default linux operating system rootfs image is resident in tmpfs as a >1GB squashfs image.
* Initial implimentation will be based upon the Xen virtalization layer, allowing dom0s other than linux to be signed and booted.
* OS-agnostic, but a certain vendor widely known as wintel likely would not boot natively. (But probably work fine as a paravirtualization guest or under HVM with qemu devices)
* Heavy reliance on running signed binaries, and allowing isolated guests to run unsigned, potentially malicious, code of their choice.
* Virtualization Guests are isolated from each other to as strong of a degree as Xen can reasonably enforce.
* OpenVSwitch will provide supervised connectivity of guests.
* Developer switch state will be read, allowing images signed under separate platform keys to be allowed to boot.
* Forethought for supporting for SteamOS as a guest, and providing high performance graphics.
* Forethought for supporting Unity3D as a "platform target", as well as running their Game Editor directly. (see http://blogs.unity3d.com/2015/08/26/unity-comes-to-linux-experimental-build-now-available/ )

Another, somewhat unrelated problem:
* Remote management with IPMI 2.0 stinks. Vendors use an ARM core, a licensed Matrox G200 2D graphics core, some fake USB 1.1 devices, and attempt to manage it all via an i2c bus variant called SMBus shared with the host.
* It could be done much better with a Freescale I.MX6, running linux, acting as a PCI Express endpoint, enabling bulk memory transfers. (see https://community.freescale.com/docs/DOC-95014 )
* Wayland 1.9.0 supports linux_dmabuf paving the way for some interesting zero-copy behavior that could be implimented. (see http://lists.freedesktop.org/archives/wayland-devel/2015-September/024303.html )
* In theory, combined with AMD's recent open source radeon driver work this could be the first solution allowing compositing from a ''Real" GPU to a mobile SoC's framebuffer memory by running the wayland protocol on both sides.
* The i.MX6 could composite additional information on top of the GPU output, in a form such as how the Steam Overlay currently operates after a Shift-Tab on PC.
* This would require additional software on both sides, but there's nothing stopping this from becoming a PCI Express card on it's own and providing remote GPU management outside of this project.

Please see the wiki for more information, or open an issue to start a discussion!
