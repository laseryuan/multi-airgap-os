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
sgdisk --zap-all /dev/sdX # may need to run twice and remove disk
lsblk
sgdisk --new=1:0:0 --typecode=1:ef00 /dev/sdX
mkfs.vfat -F32 -n GRUB2EFI /dev/sdX1
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

mv /tmp/ubuntu-16.04.2-desktop-amd64.iso /mnt/USB/
sha256sum /mnt/USB/ubuntu.iso # check file integrity
```

## Add new iso
1. download iso file
1. edit grub.cfg
1. cp ./usb-pack_efi/boot/grub/grub.cfg /mnt/USB/boot/grub/grub.cfg
1. add iso file

## References
https://www.pendrivelinux.com/boot-multiple-iso-from-usb-via-grub2-using-linux/
https://ubuntuforums.org/showthread.php?t=2276498
