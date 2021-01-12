# Inject Android Hardware USB HOST Permissions
### newbit @ xda-developers
Inject the android.hardware.usb.host.xml file to grant permissions for USB passed through devices via QEMU.

### Description
Injects the missing android.hardware.usb.host.xml into the /vendor/etc/permissions folder, to give the AVD
access to USB devices which were passed through the QEMU Emulator to the AVD.

### Links
* [XDA [GUIDE] Build / Mod AVD Kernel Android10 x86_64-29 root (Magisk) USB passthrough Linux ](https://forum.xda-developers.com/t/guide-build-mod-avd-kernel-android10-x86_64-29-root-magisk-usb-passthrough-linux.4212719/)
* [X-plore File Manager](https://play.google.com/store/apps/details?id=com.lonelycatgames.Xplore)

### Notes
The AVD QEMU Stock Kernel has only EHCI HCD (USB 2.0) Support by default.
And Since Android 11 (R) it seems to have USB Mass Storage and SCSI Device / Disk Support.
Only Apps with an own build-in USB Device Driver, like the X-plore File Manager, can handle
this Storage, and only in a limited way.
But in order to get any USB Devices, at least recognised from the Kernel,
wich is not EHCI compatible, one has to build its own AVD Kernel with additional features enabled.
* USB Serial Device (PL2303 / FTDI)
	* -> UHCI HCD (most Intel and VIA) support
* USB 3.0 Device (Pendrive/HDD)
	* -> xHCI HCD (USB 3.0) support
* Auto mount with any kind of USB Storage as a Block Device like /dev/block/sda1
	* -> SCSI Device Support
	* -> SCSI Disk Support
	* -> patched fstab.ranchu in ramdisk.img with 'echo "/devices/*/block/sd* auto auto defaults voldmanaged=usb:auto" >> fstab.ranchu' 

Without the android.hardware.usb.host.xml file, nothing of this above is possible at all.
Check your AVD with 'pm list features | grep usb' to see if it shows android.hardware.usb.host
