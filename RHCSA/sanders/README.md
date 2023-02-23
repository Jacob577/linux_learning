# These are the questions from Sanders w. answers

1.  Configure your system to automatically loop-mount the ISO of the
installation disk on the directory /repo. Configure your system to remove
this loop-mounted ISO as the only repository that is used for installation.
Do not register your system with subscription-manager, and remove all
reference to external repositories that may already exist.

```bash
mkdir repo
mount -o loop /dev/sr0 /repo
cp -v /repo/media.repo /etc/yum.repo/rhel.repo

vi /etc/yum.repo
[AppStream]
name=local RHEL AppStream
gpgcheck=0
enabled=1
baseurl=file:///repo/AppStream

[BaseOS]
name=local RHEL BaseOS
gpgcheck=0
enabled=1
baseurl=file:///repo/BaseOS

# Save
# Check if works
dnf repolist
```

<br>
2. Reboot your server. Assume that you don’t know the root password, and
use the appropriate mode to enter a root shell that doesn’t require a
password. Set the root password to mypassword.
```
# At boot, press e to disrupt the boot process
# Add:
rd.break

mount -o remount,rw /sysroot
chroot /sysroot
passwd root
# Type pwd
sync
touch /.autorelabel
exit
exit

# Now the root password should be changed
```
<br>
3. Set default values for new users. Set the default password validity to
90 days, and set the first UID that is used for new users to 2000.
```bash
vi /etc/login.defs
[...]
PASS_MAX_DAYS   90
...
UID_MIN         2000
[...]
```
- many group and user settings are located under login.defs

<br>
4. Create a 2-GiB volume group, using 8-MiB physical extents. In this volume
group, create a 500-MiB logical volume with the name mydata, and mount
it persistently on the directory /mydata.
```bash
vgcreate -s 8M -L +2G myvg /dev/sdd1
lvcreate -n mydata -L +500M myvg
mkfs.xfs /dev/myvg-mydata

# In fstab
/dev/mapper/myvg/mydata /mydata xfs defaults    0   0
```
<br>
5. Find all files that are owned by user student and copy them to the directory
/rootstudentfiles.
```bash
find / -user student -exec cp "{}" /rootstudentfiles   \;
```
<br>
6. Schedule a task that runs the command touch /etc/motd every day from
Monday through Friday at 2 a.m.
<br>
7. Add a new 10 GiB virtual disk to your virtual machine. On this disk, add
a VDO volume with a size of 20 GiB and mount it persistently.
<br>
8. Install the vsftpd service and ensure that it is started automatically at reboot.
<br>
9. Create a 1-GB XFS partition on /dev/sdb. Mount it persistently on the
directory /mydata, using the label mylabel.
<br>
10. Set default values for new users. Ensure that an empty file with the
name NEWFILE is copied to the home directory of each new user that
is created.
<br>
11. Create a 2-GiB swap partition and mount it persistently.
<br>
12. Resize the LVM logical volume that contains the root file system and
add 1 GiB.
<br>
13. Set your server to use the recommended tuned profile.
<br>
14. Create user vicky with the custom UID 2008.
<br>
15. Configure your server to synchronize time with myserver.example.com.
(Note that this server does not have to exist.)
<br>
16. Install a web server and ensure that it is started automatically.
<br>
17. Set default values for new users. Make sure that any new user password has
a length of at least six characters and must be used for at least three days
before it can be reset.
<br>
18. Create users edwin and santos and make them members of the group sales as
a secondary group membership. Also, create users serene and alex and make
them members of the group account as a secondary group.
<br>
19. Create shared group directories /groups/sales and /groups/account, and
make sure these groups meet the following requirements:
■ Members of the group sales have full access to their directory.
■ Members of the group account have full access to their directory.
■ Users have permissions to delete only their own files, but Alex is the
general manager, so user alex has access to delete all users’ files.
<br>
20. Configure a web server to use the non-default document root /webfiles. In this
directory, create a file index.html that has the contents hello world and then
test that it works. (and make it compliant with SELinux)
<br>
21. Create a 500-MiB partition on your second hard disk, and format it with
the Ext4 file system. Mount it persistently on the directory /mydata, using
the label mydata.
<br>
22. Add a 10-GiB disk to your virtual machine. On this disk, create a Stratis pool
and volume. Use the name stratisvol for the volume, and mount it persistently
on the directory /stratis.

#### Finisher:
Start a webserver using pods, it shall have persistant storage, start at boot in a non root repository. It shall use the port 7070. 