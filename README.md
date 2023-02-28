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
```

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

### Vmware vcenter: regenerate all certificates
* https://kb.vmware.com/s/article/2112283
```
Launch the vSphere 6.x Certificate Manager.

For vCenter Server 6.x/7.x Appliance: /usr/lib/vmware-vmca/bin/certificate-manager
For Windows vCenter Server 6.x: C:\Program Files\VMware\vCenter Server\vmcad\certificate-manager
 
Select Option 4 (Regenerate a new VMCA Root Certificate and replace all certificates)

Note: You can also select Option 8 (Reset all Certificates). Both options perform the same functionality. (The difference is that option 8 does not perform automatic Rollback of the certificates).

certificate vmware
 
Type the administrator@vsphere.local password when prompted.
If this is the first time VMCA certificates are re-generated on this system, you are asked to configure the certool.cfg. On subsequent tasks, you are offered to re-use these values.

Note: These values are used to define certificates issued by VMCA.

Enter these values as prompted by the VMCA (See Step 5 to confirm the Name/Hostname/VMCA):
```



## change root password
* https://www.mchelgham.com/vcsa-6-7-comment-reinitialiser-le-mot-de-passe-du-compte-root/
```
* On the ESX restart VM vcenter
* Boot on the VM when image spash photon appears 
* press 'e' 
* add in the command line 'rw init=/bin/bash' press F10
* After the boot 'loadkeys fr' , 
* tape 'passwd' and enter the password
* umount /


```

## Reset administrator@vsphere.local password
```
root@192.168.1.145's password:

[ERROR]: Failed to connect to service.
Use service-control command to manage applmgmt service

    * List APIs: "help api list"
    * List Plugins: "help pi list"
    * Launch BASH: "shell"

Command> shell
Shell access is granted to root
oot@localhost [ ~ ]# /usr/lib/vmware-vmdir/bin/vdcadmintool

==================
Please select:
0. exit
1. Test LDAP connectivity
2. Force start replication cycle
3. Reset account password
4. Set log level and mask
5. Set vmdir state
6. Get vmdir state
7. Get vmdir log level and mask
==================

3
  Please enter account UPN : administrator@vsphere.local
New password is -
8nwixxxxxxxxxxxxxxxxx
```


## Change vcenter networking
```
/opt/vmware/share/vami/vami_config_net

 Main Menu

0)	Show Current Configuration (scroll with Shift-PgUp/PgDown)
1)	Exit this program
2)	Default Gateway
3)	Hostname
4)	DNS
5)	Proxy Server
6)	IP Address Allocation for eth0
Enter a menu number [0]: 0
```
## ESX Create datacenter 

<img width="1060" alt="image" src="https://user-images.githubusercontent.com/32338685/208385523-fab10beb-32cd-4f1d-8d65-7ac0f9a20ab5.png">


## Enable ssh on ESX-i
* https://phoenixnap.com/kb/esxi-enable-ssh

<img width="764" alt="image" src="https://user-images.githubusercontent.com/32338685/221839075-15b9a5b2-9ee6-40cb-ad38-71ffe21eb8f8.png">



## Activate intel VTx
* https://www.codeproject.com/Tips/1032357/Enable-Virtualization-inside-ESXi-Virtual-Machine
```
The most tricky part. Make sure to check your steps:

    Shutdown your virtual machine inside ESXi
    SSH to your ESXi:
    C++

ssh -lroot your.esxi.box.address

run df - locate address of your mounted drive with VMs:

 df
Filesystem        Bytes         Used    Available Use% Mounted on
VMFS-5     999922073600 539996717056 459925356544  54% /vmfs/volumes/WDC1TB

Locate location of your vmx:

find / -name *.vmx | grep HUM
/vmfs/volumes/54183927-04f91918-a72a-6805ca147c55/W-NodeBox-HUM/W-NodeBox-HUM.vmx

Put setting vhv.enable=TRUE at the bottom of your box's vmx file:

 echo 'vhv.enable = "TRUE"' >> /vmfs/volumes/54183927-04f91918-a72a-6805ca147c55/W-NodeBox-HUM/
W-NodeBox-HUM.vmx

You are done.  Start your machine normally and enjoy from possibility to run vagrant&virtualbox controlled boxes inside your ESXi host. (Hopefully, you need it like me just to test the vagrant setup.)

```
