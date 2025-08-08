# Install VirtualBox Guest Additions on Ubuntu

Before installing VirtualBox Guest Additons on Ubuntu, [update](/Linux/Ubuntu/Update.md) your system.

Install build essentials (a meta-package that installs the basic tools required to compile C/C++ cod), dkms (Dynamic Kernel Module Support: A system that automatically rebuilds kernel modules (like drivers) whenever a new kernel is installed) and linux headers (the C header files for your current kernel version, if you're compiling kernel modules (e.g., for DKMS), you need headers that match your running kernel).

```bash
# $(uname -r) automatically inserts your current kernel version
$ sudo apt install build-essential dkms linux-headers-$(uname -r)
```

## Insert the Guest Additions ISO

In the VirtualBox menu bar, go to `Devices > Insert Guest Additions CD image…` Ubuntu usually auto-mounts the CD. If not, mount it manually:

> sudo mount /dev/cdrom /mnt

After mounting, run the following commands:

> sudo /mnt/VBoxLinuxAdditions.run
> sudo reboot

**If you're running a headless Ubuntu server, consider using:**

> sudo apt install virtualbox-guest-utils

## Check that the kernel modules are loaded:

lsmod | grep vbox