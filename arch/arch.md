# Installing Arch Notes
## Getting the image
- Download the image from https://archlinux.org/download/
- move the image to a usb device by using a cli tool like ``cat`` or ``dd`` or a gui tool like ``etcher`` (in this case I used etcher)
## Booting into the live boot
- Make sure __secure boot is off__ and move the usb device to the top of the boot order
- Note: on my laptop I had to set my gpu mode to ``dynamic`` instead of ``dedicated`` I assume this is an issue with nvidia drivers that I'll have to fix when I get the system running
- When booted select the first option which is ``Archlinux (x86)`` something..
- To connect to the internet run ``ip link`` to ensure the connection is good run ``ping archlinux.org``
## Setting up the clock
- To ensure the system clock is accurate run ``timedatectl set-ntp true``
## Partitioning
- Run ``fdisk -l`` to view available disks
    - the following partitions are required
    - ``root directory '/'``
    - ``efi system partition`` for booting into uefi mode
- For my partition setup I did
- 512 MB Partition (EFI)
- 512 MB Partition (Swap?)
- Rest of Disk (Root Filesystem)
### Parititoning - Formatting
- use gdisk
- Parititon Layout:
    - Boot UEFI  - 512M - FAT32
    - Grub       - 1M
    - Swap       - 16G 
    - FileSystem - Rest of Disk - ext4
### WIFI - Extra Step
- Since I'm on university wifi I had to get the mac address of my wireless device (can be seen in ``ip link``) and manually add it to my universities nomad system.
### Pacstrap issues, this fixed it
```
# mount /dev/sda1 /mnt            #The root partition.
# nano /etc/pacman.d/mirrorlist   #Choose a mirror.
# rm -rf /mnt/*                   #Start fresh.
# pacstrap /mnt base base-devel linux linux-firmware

```

### WiFI (On Installation)
- Find drivers for the given cards in the sytem and install them
- Use the built in systemd-networkd for networking. Arch wiki has good info
- Use iw for wireless networking
### Desktop Environment
- Install Xorg
- Install XFCE4
- Add startxfce to the xorg init file
