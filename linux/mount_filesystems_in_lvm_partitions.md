## Mount filesystems in LVM partitions
1) Check that LVM is installed on the system otherwise install it:
```bash
#Fedora Linux 

> sudo dnf install lvm2

#CentOS/RHEL/Oracle Linux 

> sudo yum install lvm2

#OpenSUSE/SUSE Linux 

> sudo zypper install lvm2

#Debian/Ubuntu Linux

> sudo apt install lvm2

#Arch Linux install

> sudo pacman -S lvm2
```

2) Load the device mapper module:

```bash
> sudo modprobe dm-mod
```

3) Scan the system for LVM volumes and identify the one that contains the volume needed:
```bash
> sudo vgscan 
  Found volume group "VolGrp0" using metadata type lvm2
```

4) Activate the volume:
```bash
> sudo vgchange -ay VolGrp0
```

5) Find the logical volume that contains the needed filesystem (in this case `home`):
```bash
> sudo lvs
  LV   VG      Attr       LSize    Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  home VolGrp0 -wi-ao---- <448.38g
  root VolGrp0 -wi-ao----   20.00g
  swap VolGrp0 -wc-ao----    8.00g
```

6) Prepare a mounting point for the volume:
```bash
> sudo mkdir /mount/home
```

7) Mount the volume
```bash
> sudo mount /dev/VolGrp0/home /mnt/home
```
