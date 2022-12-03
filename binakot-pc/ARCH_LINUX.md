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

## Useful tools

```bash
$ pacman -Sy sudo dhcpcd
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

### Network

```bash
# Network
$ systemctl start systemd-networkd.service
$ systemctl enable systemd-networkd.service
$ systemctl statuc systemd-networkd.service

# DNS
$ systemctl start systemd-resolved.service
$ systemctl enable systemd-resolved.service
$ systemctl statuc systemd-resolved.service

# DHCP
$ systemctl start dhcpcd.service
$ systemctl enable dhcpcd.service
$ systemctl statuc dhcpcd.service

# Interface
$ ip link
$ ip link set enp0s3 up
```

### Users and groups

```bash
$ useradd -m binakot
$ passwd binakot

$ nano /etc/sudoers
> %wheel ALL=(ALL) ALL
$ usermod -aG wheel binakot

$ exit
$ archlinux login: binakot
```

### Graphical user interface

```bash
# Terminal emulator
$ sudo pacman -Syu alacritty

# Xorg
$ sudo pacman -Syu xorg xorg-xinit xorg-drivers

# Tiling window manager
$ sudo pacman -Syu i3 dmenu ttf-dejavu
$ echo "exec i3" >> ~/.xinitrc
```

Hotkeys:

* `Alt + Enter` - new terminal
* `Alt + D` - app launcher

For autostart X at login edit `~/.bash_profile` and place the following lines:

```bash
if [[ -z $DISPLAY ]] && [[ $(tty) = /dev/tty1 ]]; then
  startx
fi
```

Generate user directories like Downloads and Documents:

```bash
$ sudo pacman -Syu xdg-user-dirs
$ xdg-user-dirs-update
```

## VirtualBox Guest Additions (Optional)

```bash
$ sudo pacman -Syu virtualbox-guest-utils
$ sudo systemctl enable vboxservice.service
$ sudo reboot
```

Ref: https://youtu.be/hmku7eW8UFg

## Applications

### Arch User Repository Helper

```bash
$ sudo pacman -S --needed git base-devel
$ git clone https://aur.archlinux.org/yay-bin.git
$ cd yay-bin
$ makepkg -si
```

### Apps

```bash
$ yay google-chrome
```
