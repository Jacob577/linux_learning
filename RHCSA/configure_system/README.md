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
Look above


### Extend existing logical volumes
Extending a volume isn't terrificly dificult, unmount the volume, make sure the file systems are the same. Then `lvextend -L <size> <path/to/LV> <path/to/PV>`
Or if the LV is mounted on a VG, just `lvextend <LV/path> -L <size>`

Side note, lvextend has a great manual for related commands, have a look there if something else is unclear! 
### Create and configure set-GID directories for collaboration
[Great resource](https://linuxconfig.org/create-and-configure-setgid-directories-for-collaboration-rhcsa-objective-preparation)

If we'd like to create a collab directory:
```bash
mkdir -p /shared/collab

useradd john
useradd doe

groupadd collaborators
usermod -aG collaborators john doe

chown -R :collaborators /shared/collab

chmod 770 /shared/collab

# We could add the -R flag to apply permissions to subsequent directories
# Thereafter we can add the setgid flag to permissions:
chmod g+s /shared/collab

# Now they can collaborate properly on projects within this directory!
```

### Diagnose and correct file permission problems