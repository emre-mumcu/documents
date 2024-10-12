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