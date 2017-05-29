## GRUB2
### Prepare
wget http://releases.ubuntu.com/16.04/ubuntu-16.04.2-desktop-amd64.iso -P /tmp/

Get usb device name /dev/sdX by using the command:  
lsblk

### Format the usb
```
apt-get install gdisk
sudo -i
lsblk
umount /dev/sdX1
sgdisk -p /dev/sdX
```

> Disk /dev/sdl: 31260672 sectors, 14.9 GiB
> Logical sector size: 512 bytes
> Disk identifier (GUID): DBEC6655-5508-485D-BABF-954C8C2E46B0
> Partition table holds up to 128 entries
> First usable sector is 34, last usable sector is 31260638
> Partitions will be aligned on 2048-sector boundaries
> Total free space is 2014 sectors (1007.0 KiB)

Size to sector number: 1KB: 2 sectors 1GB: 1024x1024x2=2097152 sectors
Sector range: 34-31260638

```
sgdisk --zap-all /dev/sdX # destroy mbr/gpt, and create new gpt. may need to run twice and remove disk
sgdisk --new=1:2048:10485760 --new=2:10485761:31260638 --typecode=1:ef00 --typecode=2:8300 -g -p /dev/sdX # 5GB (10485760 sectors) + 10 GB

mkfs.vfat -F32 -n GRUB2EFI /dev/sdX1
mkfs.ext4 /dev/sdX2
exit
```

### Mount the usb
```
mkdir /mnt/USB # if the directory does not exist
sudo mount -t vfat /dev/sdX1 /mnt/USB -o uid=1000,gid=1000,umask=022
```

### Settup grub2
```
rsync -auv ./usb-pack_efi/ /mnt/USB
sudo grub-install --force --removable --boot-directory=/mnt/USB/boot --efi-directory=/mnt/USB/EFI/BOOT /dev/sdX

wget https://stable.release.core-os.net/amd64-usr/current/coreos_production_iso_image.iso -P {iso_folder}
sha256sum { ubuntu_iso } # check file integrity
```

### Add new iso
1. download iso file
1. edit grub.cfg
1. cp ./usb-pack_efi/boot/grub/grub.cfg /mnt/USB/boot/grub/grub.cfg
1. add iso file

### fix fat partition
sudo fsck.vfat -a /dev/sdX1

## Virtrualbox
umount the drive before start virtualbox

## Docker
mkdir /media/livecd
sudo mount /dev/sda2 /media/livecd
/media/livecd/bootstrap.sh

## References
https://www.pendrivelinux.com/boot-multiple-iso-from-usb-via-grub2-using-linux/
https://ubuntuforums.org/showthread.php?t=2276498
