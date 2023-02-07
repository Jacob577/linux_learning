## Configure local storage

- List, create, delete partitions on MBR and GPT disks
- Create and remove physical volumes
- Assign physical volumes to volume groups
- Create and delete logical volumes
- Configure systems to mount file systems at boot by universally unique ID (UUID) or label
- Add new partitions and logical volumes, and swap to a system non-destructively

### List, create, delete partitions on MBR and GPT disks

### Create and remove physical volumes
Create physical volume:
```bash
# Create partition
fdisk <disk>

# Create physical volume
pvcreate <path/to/partition>

# Remove partitions
pvremove <path/to/partition>

# Remove partition
fdisk <path/to/disk>
Press: d & w
```
### Assign physical volumes to volume groups
The manual for `vgcreate` is great and how you go about assigning a physical volume to a volume group is:
```bash
vgcreate <name of vg> <path/to/physical/volume>
```

### Create and delete logical volumes
How do you create a logical volume from a volume group?
```bash
# Create logical volume
lvcreate -L <size> --name <name> <name of VG>

# Remove logical volume
lvremove <path/to/LV>
```

### Configure systems to mount file systems at boot by universally unique ID (UUID) or label
To configure an xfs fs to mount at boot:
```bash
mkfs.xfs <path/to/lv>
xfs_admin -L <label> <path/to/fs>

# How I got the UUID to fstab:
blkid | grep -ni <label> | awk '{print $3}' >> /etc/fstab

# Then ins fstab, just add the rest of the parameters
```

### Add new partitions and logical volumes, and swap to a system non-destructively
How to setup a new swap
```bash
swapoff
fdisk <create the new partition>
mkswap <path/to/new/partition>
blkid | grep -ni <partition> | awk '{print $2}' >> /etc/fstab
# Add the rest in fstab

# I would suggest using mount -a since we'll get an error if fstab is faulty
mount -a 
```