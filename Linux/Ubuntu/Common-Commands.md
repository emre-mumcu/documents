# Ubuntu Common Commands

## update

It updates the local package list (metadata) with the latest information about packages and versions from the repositories.

It does not install or upgrade any software packages. It does NOT change system.

> sudo apt update

## upgrade

It upgrades all the installed packages on your system to the latest versions, but only if no new packages need to be installed or removed.

If a newer version of a package requires adding/removing dependencies, apt upgrade will skip it to avoid breaking anything.

It does change system safely.

> sudo apt upgrade

## full-upgrade

This command is like upgrade command, but it can add or remove packages to complete the upgrade.

It does change system aggressively.

> sudo apt full-upgrade

## update && upgrade

sudo apt update && sudo apt upgrade -y

## autoremove

Clean unused packages:

> sudo apt autoremove

## do-release-upgrade

Release Upgrade Process. This will upgrade to the next available release:

> sudo do-release-upgrade