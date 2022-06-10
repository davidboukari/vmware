# vmware-esxi

## Create an usb boot disk

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

* Finally there are 3 partitions on the USB disk  
  => BOOTBANK1: the esxi boot files like b.b00, crx.v00, ...
  => BOOTBANK2: boot.cfg
  => unknown: the esxi real partition

## Install vmware-tools

```bash
sudo -s
yum install perl
yum install open-vm-tools
tar xzvf VMwareTools-10.3.22-15902021.tar.gz
cd vmware-tools-distrib
./vmware-install.pl
```

## Install VCenter
* mount the VMware-VCSA-all-7.0.1-17004997.iso
* exec ui installer
* Go to https://vsphereIP
* create:  Add new DC -> Add new Cluster -> Add Host


# Deploy OVA to Template
```
Go to https://www.vmware.com/go/get-tkg and log in with your My VMware credentials.

Download the Tanzu Kubernetes Grid OVAs for node VMs.

    Kubernetes 1.18.6: Photon v3 Kubernetes 1.18.6 OVA
    Kubernetes 1.17.9: Photon v3 Kubernetes 1.17.9 OVA

If you want to use Tanzu Kubernetes Grid 1.1.3 to deploy Kubernetes clusters with older versions as well as Kubernetes v1.18.6 and v1.17.9 clusters, after downloading the OVAs above, select version 1.1.2, 1.1.0, or 1.0.0 in the downloads page, and download the Photon v3 Kubernetes OVAs for those releases.
In the vSphere Client, right-click an object in the vCenter Server inventory, select Deploy OVF template.
Select Local file, click the button to upload files, and navigate to the downloaded OVA file on your local machine.

Follow the installer prompts to deploy a VM from the OVA temaplate.

    Accept or modify the appliance name
    Select the destination datacenter or folder
    Select the destination host, cluster, or resource pool
    Accept the end user license agreements (EULA)
    Select the disk format and destination datastore
    Select the network for the VM to connect to

NOTE: If you select thick provisioning as the disk format, when Tanzu Kubernetes Grid creates cluster node VMs from the template, the full size of each node's disk will be reserved. This can rapidly consume storage if you deploy many clusters or clusters with many nodes. However, if you select thin provisioning, as you deploy clusters this can give a false impression of the amount of storage that is available. If you select thin provisioning, there might be enough storage available at the time that you deploy clusters, but storage might run out as the clusters run and accumulate data.
Click Finish to deploy the VM.

When the OVA deployment finishes, right-click the VM and select Template > Convert to Template.

NOTE: Do not power on the VM before you convert it to a template.

In the VMs and Templates view, right-click the new template, select Add Permission, and assign the tkg-user to the template with the TKG role.


## Troubleshooting
```
Error Message on boot:
CRC error during decompression. Receiveid CRC ... gzip_extract failed for vim.v00
....


=> Copy the file vim.v00 from the cdrom ESXI-7.....-STANDARD to the USB partition BOOTBANK1
Then put the usb disk on the server and start it


```

* setting ip/ipv6 configuration failed: ('
http://vcloud-lab.com/entries/vcenter-server/vmware-vcenter-server-vcsa-setting-ip-ipv6-configuration-failed-ip-configuration-not-allowed

* Unable to connect to https://vcenter:5480 after ip change
https://communities.vmware.com/t5/VMware-vCenter-Discussions/vCenter-6-fails-to-restart-after-IP-change/td-p/418957
```
I ran into this issue today when changing the ip-address of the vcenter appliance. It seems the old ip address is not changed in the hosts file on the appliance. to correct this login to appliance management on port 5480 and enable ssh login & bash access. login with ssh and drop into the shell, edit the /etc/hosts file with vi and change the ip-address. After a reboot of the VM and restarting all services in the shell everything was working again.
```

