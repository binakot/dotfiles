# Arch Linux

https://archlinux.org/download/

https://wiki.archlinux.org/title/Installation_guide

## Pre-installation

```bash
# Connect to the internet
$ ip link
$ ping archlinux.org

# Update the system clock
$  timedatectl status

# Partition the disks
$ fdisk -l
$ fdisk /dev/sda
> o
> n -> p -> 1 -> default -> +2G
> n -> p -> 2 -> default -> default
> t -> 1 -> swap
> a -> 2
> w
$ fdisk -l /dev/sda

# Format the partitions
$ mkswap /dev/sda1
$ mkfs.ext4 /dev/sda2

# Mount the file systems
$ swapon /dev/sda1
$ mount /dev/sda2 /mnt
```
