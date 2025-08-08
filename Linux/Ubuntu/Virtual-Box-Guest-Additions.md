# Install VirtualBox Guest Additions on Ubuntu

Before installing VirtualBox Guest Additons on Ubuntu, [update](/Linux/Ubuntu/Update.md) your system.






sudo apt update && sudo apt upgrade -y

sudo apt install build-essential dkms linux-headers-$(uname -r)

## Insert the Guest Additions ISO

In the VirtualBox menu bar, go to Devices > Insert Guest Additions CD image…

Ubuntu usually auto-mounts the CD. If not, mount it manually:

sudo mount /dev/cdrom /mnt

sudo /mnt/VBoxLinuxAdditions.run

sudo reboot

**If you're running a headless Ubuntu server, consider using:**

sudo apt install virtualbox-guest-utils

## Check that the kernel modules are loaded:

lsmod | grep vbox