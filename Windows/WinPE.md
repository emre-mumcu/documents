Create bootable Windows PE media

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/winpe-create-usb-bootable-drive?view=windows-11

Make sure your PC has the ADK and ADK Windows PE add-on installed.

1. Download the Windows ADK

Go to the Windows ADK download page and download the version that matches your Windows version.
https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install

Run the ADK installer (aksetup.exe) and choose Deployment Tools and Windows Preinstallation Environment (WinPE) during installation.

Run adkwinpesetup.exe to install the Windows PE add-on

Start the Deployment and Imaging Tools Environment as an administrator.

2. Create a WinPE Bootable USB Drive (gerek yok)

diskpart
list disk
select disk 1
clean
create partition primary
select partition 1
active
format fs=ntfs quick
assign letter=Z
exit

3. Stage the WinPE Files (C:\WinPE is stage folder, folder will be created by copype command)

copype amd64 C:\WinPE

4.a. Make a bootable USB (Z is USB drive letter)

makewinpemedia /ufd C:\WinPE Z:

4.b. Make a bootable Windows PE ISO image

makewinpemedia /ISO C:\WinPE C:\WinPEx64.iso

5. Optional: Customize the WinPE Environment

Mount the WinPE image to add extra files:

Cretae C:\mount directory before running the command:

dism /mount-wim /wimfile=C:\WinPE\media\sources\boot.wim /index:1 /mountdir=C:\mount

dism /mount-wim /wimfile=C:\WinPE\media\sources\boot.wim /index:1 /mountdir=C:\WinPE\mount

Copy your files (e.g., disk utilities, drivers, scripts) to the mounted image at C:\mount. Commit the changes and unmount the image:

dism /unmount-wim /mountdir:C:\mount /commit
dism /unmount-wim /mountdir:C:\WinPE\mount /commit

After that, re-run step 4a or 4b








