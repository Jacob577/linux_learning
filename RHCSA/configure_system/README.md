## Create and configure file systems

- Create, mount, unmount, and use vfat, ext4, and xfs file systems
- Mount and unmount network file systems using NFS
- Configure autofs
- Extend existing logical volumes
- Create and configure set-GID directories for collaboration
- Diagnose and correct file permission problems

### Create, mount, unmount, and use vfat, ext4, and xfs file systems
To create file systems we can either use fdisk and allocate the correct file system to the partition directly by pressing `t`, `l` to show what file types are available. 
Otherwise, we can use `mkfs.ext4` etc... 

To manually mount file systems, we can use `mount -t <file system>`

To further unmount disks or file systems, we can use umount. 
### Mount and unmount network file systems using NFS
In order to mount NFS drives, we need nfs-utils installed. 
We mount it with:
[Credit - Linuxize](https://linuxize.com/post/how-to-mount-an-nfs-share-in-linux/)
```bash
mount -t nfs <ip to NFS>:<path> <target/path/locally>

# In fstab
# <file system>     <dir>       <type>   <options>   <dump>	<pass>
10.10.0.10:/backups /var/backups  nfs      defaults    0       0
```

A littel complicated so I'll wait untill I've done some exercises with autofs
[How to mount NFS using autofs](https://www.linuxtechi.com/automount-nfs-share-in-linux-using-autofs/)
But here is how one should go about mounting NFS with autofs
```bash
# dnf install autofs (if not installed)
# check with rpm -qa | grep autofs

# Edit /etc/auto.master
/mount-point    /etc/<auto.map> --timeout=180

# Create the map file
vi /etc/auto.map
<NFS-target name>   -fstype=nfs,rw,soft,intr    <ip to NFS>:/<NFS-target/path>

# Start autofs service
systemctl enable --now autofs.service

```
### Configure autofs

### Extend existing logical volumes
Extending a volume isn't terrificly dificult, unmount the volume, make sure the file systems are the same. th
### Create and configure set-GID directories for collaboration
### Diagnose and correct file permission problems