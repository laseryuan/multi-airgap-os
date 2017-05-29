1. download iso file
wget https://stable.release.core-os.net/amd64-usr/current/coreos_production_iso_image.iso -P /tmp/

1. edit grub.cfg

1. update grub.cfg
cp ./usb-pack_efi/boot/grub/grub.cfg /mnt/USB/boot/grub/grub.cfg

1. add iso file
mv /tmp/coreos_production_iso_image.iso /mnt/USB/iso/
