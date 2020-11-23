# vmware-esxi

* https://www.altaro.com/vmware/esxi-usb-flash-drive-linux/

```bash
yum install syslinux

fdisk /dev/sdc
* Press [d] to delete any existing partitions. (Optional step).
* Press [n], [p] and press Enter 3 times to accept the default settings. This step creates a primary partition for you.
* Press [t] to toggle the partition file system type.
* Press [c] to set the file system type to FAT32
* Press [a] to make the partition active.
* Press [w] to write the changes to disk.

/sbin/mkfs.vfat -F 32 -n USBESXi /dev/sdc1 

/usr/bin/syslinux /dev/sdc1

cat /usr/share/syslinux/mbr.bin > /dev/sdc
or
dpkg-query -L syslinux
cat /usr/lib/SYSLINUX/mbr.bin > /dev/sdf

mkdir /tmp/usbdisk
mount /dev/sdc1 /tmp/usbdisk
mkdir /tmp/esxicd
mount -o loop VMware-VMvisor-Installer-7.0U1-16850804.x86_64.iso /tmp/esxicd
ls /tmp/esxicd/
time cp -r /tmp/esxicd/* /tmp/usbdisk
mv /tmp/usbdisk/isolinux.cfg /tmp/usbdisk/syslinux.cfg

vi /usbdisk/syslinux.cfg  (add -p 1)
  APPEND -c boot.cfg -p 1    
```
