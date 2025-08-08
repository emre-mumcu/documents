# Kernel Modules

Kernel modules are pieces of code that can be loaded into the Linux kernel at runtime, without the need to reboot or recompile the kernel.

Think of them as plug-ins for the Linux kernel—they extend the kernel’s functionality in a modular way.

**Examples of Kernel Modules:**

* Device drivers: for network cards, USB devices, file systems, etc.
* VirtualBox guest drivers: like vboxguest, vboxsf, vboxvideo (for Guest Additions)
* File systems: e.g., nfs, btrfs, fuse
* Security modules: like AppArmor or SELinux

## Useful Commands for Working with Modules

| Command                 | Description                          |
| ----------------------- | ------------------------------------ |
| `lsmod`                 | List currently loaded kernel modules |
| `modprobe <modulename>` | Load a module                        |
| `rmmod <modulename>`    | Unload a module                      |
| `modinfo <modulename>`  | Show information about a module      |

> *Example:* Check if VirtualBox modules are loaded:

```bash
$ lsmod | grep vbox
```