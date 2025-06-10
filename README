# Dot files

## Arch Linux Install

Verifying boot mode (Should return 64)
`# cat /sys/firmware/efi/fw_platform_size`

Set time and date
```
# timedatectl set-timezone Region/City
# timedatectl status
```

Create partitions for Arch Linux:
- /boot - Around 1GiB (EFI system partition)
- [SWAP] - Amount of RAM + 1-2Gib (Linux swap)
- / - At least 32 GiB (Linux filesystem)

```
# lsblk
# cfdisk /dev/xyz
```

Format partitions:

```
# mkfs.ext4 /dev/rootPartition
# mkfs.fat -F 32 /dev/efiPartition
# mkswap /dev/swapPartition
```

Mount partition:

```
# mount /dev/rootPartition /mnt
# mount --mkdir /dev/efiPartition /mnt/boot
# swapon /dev/swapPartition
# genfstab -U /mnt >> /mnt/etc/fstab
```

Install packages using pacstrap

`# pacstrap -K /mnt base base-devel linux linux-firmware linux-headers intel-ucode networkmanager neovim sudo`

Chroot into system
```
# arch-chroot /mnt
# ln -sf /usr/share/zoneinfo/Region/City /etc/localtime
# hwclock --systohc
```

Edit `/etc/locale.gen`, uncomment `en_US.UTF-8 UTF-8`, then run `# locale-gen` \
Create `/etc/locale.conf`, then echo `LANG=en_US.UTF-8` into it. \
Create `/etc/hostname`, then echo your hostname into it.

Download Grub, sometimes you need to also create `/boot/EFI/BOOT/` and copy the file `/boot/EFI/GRUB/grubx64.efi`, and rename it to `/boot/EFI/GRUB/bootx64.efi`

```
# pacman -S grub efibootmgr
# grub-install --target=x86-64-efi --efi-directory=/boot --bootloader-id=GRUB
# grub-mkconfig -o /boot/grub/grub.cfg
```

Create new user

```
# useradd -m -g users -G wheel name
# passwd name
```

Edit visudo with `# EDITOR=nvim visudo`
`%wheel ALL=(ALL:ALL) ALL`

Change root password

`# passwd`

Initramfs, leave chroot.
```
# mkinitcpio -P
# exit
# umount -R /mnt
# reboot
```

After first boot enable `NetworkManager`
```
$ sudo systemctl enable NetworkManager
$ sudo systemctl start NetworkManager
```

Download nvidia drivers for nvidia cards (https://wiki.archlinux.org/title/NVIDIA), pick the package for your card.

`$ sudo pacman -Syu nvidia`

Edit kernel parameters in `/etc/default/grub`, then `$ sudo grub-mkconfig -o /boot/grub/grub.cfg`

`GRUB_CMDLINE_LINUX_DEFAULT="loglevel=3 splash nvidia_drm.modeset=1 nvidia_drm.fbdev=0`






