# Arch Linux Manual Installation Report

## Objective

Perform a manual installation of Arch Linux with disk partitioning, base system installation, GRUB bootloader configuration, network services setup, and GNOME desktop environment installation.

---

# Procedure

## 1. Network Connectivity Check

```bash
ping archlinux.org
```

Purpose:
- verify internet connection;
- check DNS availability.

---

## 2. IP Address Check

```bash
ip addr show
```

Purpose:
- display network interfaces;
- obtain the system IP address for possible SSH access.

---

## 3. Time Synchronization

```bash
timedatectl
```

Purpose:
- synchronize system time;
- verify time service status.

---

## 4. Disk Detection

```bash
fdisk -l
```

Purpose:
- display connected storage devices;
- identify the target disk for installation.

---

# Disk Partitioning

Selected disk:

```bash
fdisk /dev/sdx
```

Actions performed inside `fdisk`:

## Creating Swap Partition

```bash
n
p
1

+2G
```

## Creating System Partition

```bash
n
p
2
```

## Changing Swap Partition Type

```bash
t
1
82
```

## Checking Partition Table

```bash
p
```

## Saving Changes

```bash
w
```

---

# Formatting Partitions

## Formatting System Partition

```bash
mkfs.ext4 /dev/sda2
```

## Formatting Swap Partition

```bash
mkswap /dev/sda1
```

---

# Mounting Partitions

## Mounting System Partition

```bash
mount /dev/sda2 /mnt
```

## Enabling Swap

```bash
swapon /dev/sda1
```

---

# Base System Installation

```bash
pacstrap -K /mnt base linux linux-firmware sudo micro
```

---

# Generating fstab

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

---

# Entering Installed System

```bash
arch-chroot /mnt
```

---

# Time Configuration

## Setting Time Zone

```bash
ln -sf /usr/share/zoneinfo/Russia/Moscow /etc/localtime
```

## Synchronizing Hardware Clock

```bash
hwclock --systohc
```

---

# Installing Required Packages

```bash
pacman -S dhcpcd iwd networkmanager grub gnome
```

---

# Localization Setup

## Generating Locales

```bash
locale-gen
```

## Setting System Language

```bash
echo "LANG=en_US.UTF-8" > /etc/locale.conf
```

---

# Hostname Configuration

```bash
echo archlinux > /etc/hostname
```

---

# Creating initramfs

```bash
mkinitcpio -P
```

---

# Setting Root Password

```bash
passwd
```

---

# GRUB Bootloader Installation

## Installing GRUB

```bash
grub-install --target=i386-pc /dev/sda
```

## Generating GRUB Configuration

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

---

# Enabling System Services

## DHCP Client

```bash
systemctl enable dhcpcd
```

## NetworkManager

```bash
systemctl enable NetworkManager
```

## Wi-Fi Service

```bash
systemctl enable iwd
```

## GNOME Display Manager

```bash
systemctl enable gdm
```

## SSH Service

```bash
systemctl enable sshd
```

---

# User Creation

## Creating a New User

```bash
useradd -m username
```

## Setting User Password

```bash
passwd username
```

## Adding User to Groups

```bash
usermod -aG wheel,audio,video,optical,storage,input username
```

---

# sudo Configuration

Open the file:

```bash
micro /etc/sudoers
```

Uncomment the following line:

```bash
# %wheel ALL=(ALL:ALL) ALL
```

Result:

```bash
%wheel ALL=(ALL:ALL) ALL
```

---

# Finishing Installation

## Exiting chroot

```bash
exit
```

## Unmounting Partitions

```bash
umount -R /mnt
```

## Rebooting the System

```bash
reboot
```

---

# Result

During the installation process:
- the disk was partitioned;
- a swap partition was created;
- the base system was installed;
- the GRUB bootloader was configured;
- network services were installed;
- the GNOME desktop environment was installed;
- a user account and sudo privileges were configured.

The system was successfully prepared for further use.
