# touch

```zsh
# Create an empty file
% touch filename.txt

# Edit the file content
% echo "Your text here" > filename.txt
```

# curl

```zsh
% curl -LI ***
```

# diskutil

```zsh
% diskutil list
% diskutil listFilesystems
% diskutil list disk1
% diskutil info disk1s1

# Erase Disk
% diskutil eraseDisk {filesystem} {Name to use} /dev/{disk identifier}
% diskutil eraseDisk free "Untitled" /dev/diskX

# Disk Identifier: Only the primary part of the identifier (i.e. disk1, disk2, disk3...) is needed. The additional segment indicating the partition number is omitted.

# diskpart clean
% diskutil zeroDisk short diskX

# Unmount Disk
diskutil unmountDisk /dev/diskX

# Eject Disk
diskutil eject /dev/diskX
```



To create a bootable USB drive from an ISO file on macOS using the dd command, follow these steps:

% diskutil list

Identify your USB drive in the list. It will be something like /dev/diskX (make sure you're selecting the correct one).

Unmount the USB drive (but don’t eject it):

% diskutil unmountDisk /dev/diskX

Run the dd command to copy the ISO to the USB drive.

% sudo dd if=/path/to/your.iso of=/dev/diskX bs=1m

if= is the input file (your ISO file).
of= is the output file (the raw device of your USB drive).
bs=1m sets the block size to 1 MB, which is typically faster.

After the dd command finishes, make sure all writes are flushed to the USB drive

% sudo sync

Finally, eject the USB drive

% diskutil eject /dev/diskX

