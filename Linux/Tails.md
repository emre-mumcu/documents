Install Tails using dd
----------------------

Make sure that the USB stick on which you want to install Tails is unplugged. Execute the following command:

ls -1 /dev/sd?

It returns a list of the storage devices on the system. For example:
dev/sda

Plug in the USB stick on which you want to install Tails. Execute again the same command:

ls -1 /dev/sd?

Your USB stick appears as a new device in the list.

/dev/sda
/dev/sdb

In this example, the device name of the USB stick is /dev/sdb. Yours might be different.

Execute the following commands to copy the USB image that you downloaded earlier to the USB stick.

$ dd if=/home/tails.img of=/dev/sdb bs=16M oflag=direct status=progres

If you get a Permission denied error, try adding sudo at the beginning of the command: sudo dd 


Install Tails using GNOME Disks
-------------------------------

If you are using the GNOME desktop environment, GNOME Disks should be installed by default. Otherwise, install the gnome-disk-utility package using the usual installation method for your distribution.

Start GNOME Disks (also called Disks).

Plug in the USB stick on which you want to install Tails.

A new drive appears in the left pane. Click on it.

Click on the Drive Options button in the title bar and choose Restore Disk Image.

In the Restore Disk Image dialog, click on the file selector button. Choose the USB image that you downloaded earlier.

Click the Start Restoring button.

In the confirmation dialog click Restore.