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

## Installation

```bash
# Install essential packages
$ pacstrap -K /mnt base linux linux-firmware
```

## Configure the system

```bash
# Fstab
$ genfstab -U /mnt >> /mnt/etc/fstab
$ cat /mnt/etc/fstab

# Chroot
$ arch-chroot /mnt

# Time zone
$ ln -sf /usr/share/zoneinfo/Europe/Moscow /etc/localtime
$ hwclock --systohc

# Localization
$ pacman -Sy nano
$ nano /etc/locale.gen
> en_US.UTF-8 UTF-8
> ru_RU.UTF-8 UTF-8
$ locale-gen
$ echo "LANG=en_US.UTF-8" >> /etc/locale.conf

# Network configuration
$ echo "archlinux" >> /etc/hostname

# Root password
$ passwd
> toor

# Boot loader
$ pacman -Sy intel-ucode amd-ucode
$ pacman -Sy grub
$ grub-install --target=i386-pc /dev/sda
$ grub-mkconfig -o /boot/grub/grub.cfg
```

## Reboot

```bash
$ exit
$ umount -R /mnt
$ reboot
```

## Post-installation

```bash
$ archlinux login: root
> toor
$ uname -a
```

https://wiki.archlinux.org/title/General_recommendations
