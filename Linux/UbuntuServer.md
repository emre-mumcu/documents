







## Get IP Address

```bash
$ ip a
$ ip addr show
$ hostname -I
```

## UFW (Uncomplicated Firewall) Firewall

On Ubuntu, the default and most commonly used firewall is UFW (Uncomplicated Firewall).

```bash
$ sudo ufw status
$ sudo ufw status verbose
$ sudo ufw enable
$ sudo ufw disable

## SSH
$ sudo ufw allow ssh
## or specify port
$ sudo ufw allow 22/tcp

$ sudo ufw allow http
$ sudo ufw allow 80/tcp

$ sudo ufw allow https
$ sudo ufw allow 443/tcp

$ sudo ufw deny 80
```

## Update Ubuntu Server

```bash
## This fetches the latest info about available updates:
$ sudo apt update
## This installs the available updates:
$ sudo apt upgrade

## Full upgrade (handles package removals and installs if necessary):
$ sudo apt full-upgrade

## Remove unused dependencies:
$ sudo apt autoremove

## Clean package cache (optional):
$ sudo apt clean

## Check for available distribution upgrades (e.g., Ubuntu 22.04 → 24.04)
## (Use this only if you intend to upgrade to a new Ubuntu version)
$ sudo do-release-upgrade

## Preview available updates without installing
$ apt list --upgradable
```

## Useful Commands

```bash
## Shutdown
$ sudo init 0
$ sudo shutdown now
## Shutdown after 1 minute (default):
$ sudo shutdown
## Schedule shutdown (e.g., after 5 minutes):
$ sudo shutdown +5
## With a custom message:
$ sudo shutdown +10 "System will shut down in 10 minutes for maintenance."

## Restart
$ sudo init 6
$ sudo reboot
$ sudo shutdown -r now
## Schedule a restart in 5 minutes:
$ sudo shutdown -r +5

## Cancel a scheduled shutdown/restart
$ sudo shutdown -c
```

## Some Useful Packages

```bash
$ sudo apt install net-tools
```