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
```bash
crontab -u root -e
0 14 * * 1-5 root bash touch /etc/motd
```
<br>
7. Add a new 10 GiB virtual disk to your virtual machine. On this disk, add
a VDO volume with a size of 20 GiB and mount it persistently.
```bash
# To see the available disks
lsblk
vdo create --name=vdo0 --device=/dev/sdc --vdoLogicalSize=20G

# Check VDO details
vdostats

mkdir /myvdo

# Copy over the unit file example for VDOs
cp -v /usr/share/doc/vdo/examples/systemd/VDO.mount.example /etc/systemd/system/vdo0.mount

# Modify the unit file accordingly
# Remember that the name of the unit file needs to have the same name as mount point
systemctl enable --now vdo0.mount

# or add to fstab
man vdo | grep -ni "/dev/mapper/vdo0 /vdo" >> /etc/fstab
vi /etc/fstab
# Remove bloat in entry

# Check so that the VDO is mounted correctly
lsblk
```
<br>
8. Install the vsftpd service and ensure that it is started automatically at reboot.
```bash
dnf install vsftp 

systemctl enable --now vsfpd
```
<br>
9. Create a 1-GB XFS partition on /dev/sdb. Mount it persistently on the
directory /mydata, using the label mylabel.
```bash
fdisk /dev/sdd
mkfs.xfs -L mydata /dev/sdd2

# In fstab:
/dev/sdd2   /myxfs  xfs defaults    0   0

mount -a
```
<br>
10. Set default values for new users. Ensure that an empty file with the
name NEWFILE is copied to the home directory of each new user that
is created.
```bash
cd /etc/skel
touch NEWFILE
```
<br>
11. Create a 2-GiB swap partition and mount it persistently.
```bash
fdisk /dev/sdd
# Choose to format as swap

mkswap /dev/sdd3
swapoff

# Swap on manually
swapon /dev/sdd3

# In fstab, comment out previous swap
/dev/sdd3   none    swap    defaults    0   0
swapon
```
<br>
12. Resize the LVM logical volume that contains the root file system and
add 1 GiB.
```bash
lvextend -L +1G /dev/myvg/mydata
```
<br>
13. Set your server to use the recommended tuned profile.
```bash
tuned-adm recommend
(virtual)

tuned-adm profile virtual-guest
```
<br>
14. Create user vicky with the custom UID 2008.
```bash
useradd -u 2008 vicky

# Check:
cat /etc/passwd | grep -ni vicky
```
<br>
15. Configure your server to synchronize time with myserver.example.com.
(Note that this server does not have to exist.)
```bash
timedatectl set-ntp true
firewall-cmd --add-service=ntp --permanent
firewall-cmd --reload

# Go to chrony.conf
vi /etc/chrony.conf

# At the very top, there should say pool or server, something similar there we add the ip
server pool.ntp.org

# Or in our case:
server myserver.example.com iburst

systemctl stop/disable ntp
systemctl enable --now chronyd

# To get into chronys interactive, use
chronyc

# To see what servers we are connected to in chronyc:
sources
```

<br>
16. Install a web server and ensure that it is started automatically.
```bash
dnf install httpd -y
firewall-cmd --permanent --add-service=http
firewall-cmd --permanent --add-port=80/tcp (can also add 8080)
firewall-cmd --reload

systemctl enable --now httpd

```
<br>
17. Set default values for new users. Make sure that any new user password has
a length of at least six characters and must be used for at least three days
before it can be reset.
```bash
vi /etc/login.defs

PASS_MIN_SAYS   3
PASS_MIN_LEN    6
```
<br>
18. Create users edwin and santos and make them members of the group sales as
a secondary group membership. Also, create users serene and alex and make
them members of the group account as a secondary group.
```bash
groupadd sales
groupadd account

useradd -g account serena
useradd -g account alex
useradd -g sales edwin
useradd -g sales santos
```
<br>
19. Create shared group directories /groups/sales and /groups/account, and
make sure these groups meet the following requirements:
■ Members of the group sales have full access to their directory.
■ Members of the group account have full access to their directory.
■ Users have permissions to delete only their own files, but Alex is the
general manager, so user alex has access to delete all users’ files.
```bash
mkdir -p /groups/sales
mkdir -p /groups/account

chown nobody:sales /groups/sales
chown nobody:account /groups/account

chown alex:sales /groups/sales
chown alex:account /group/account

chmod -R ug+rwx account
chmod -R ug+rwx sales

chmod -R o+t account
chmod -R o+t sales
```
<br>
20. Configure a web server to use the non-default document root /webfiles. In this
directory, create a file index.html that has the contents hello world and then
test that it works. (and make it compliant with SELinux)
```bash
# Install httpd and configure firewall & SELinux

semanage fcontext -a -t httpd_sys_content_t "/directory(/.*)?"
restorecon -R -v /directory

vi /etc/httpd/conf/httpd.conf
[...]
DocumentRoot "/directory"
[...]

systemctl enable --now httpd
systemctl restart httpd
```
<br>
21. Add a 10-GiB disk to your virtual machine. On this disk, create a Stratis pool
and volume. Use the name stratisvol for the volume, and mount it persistently
on the directory /stratis.
```bash
dnf install stratisd
dnf install stratis-cli

systemctl enable --now stratisd

stratis pool create stratis_pool /dev/sdb
stratis filesystem create stratis_pool stratis_fs

# Easy way of adding stratis vol to fstab
stratis pool list
echo "path" >> /etc/fstab

# In fstab
"path" /mount-point xfs x-systemd.requires=stratisd.service 0   0
mount -a
```

### Difficulties:
I found it difficult utilizing all of the functionality from find, but now I know there is a flag, `--exec` which makes it possible to execute commands with the results from find. We can search for permissions `--perm`, or owner `--users`. But also just `--name`.

When mounting a VDO, we need to name the .mount same as the directory it shall be mounted on on the unit file.

When mounting a stratis vol; it is important to not use `defaults`, I shall use: `x-systemd.requires=stratisd.service`. There is a hint of this in `man vdo`. 

To add a time service, add it to `/etc/chrony.conf` as a server, we can add `burst` or `iburst`, though `iburst` can be considered aggressive and may be prohibited by some time services. To check if the servicce has been registred; `chronyc` --> `source`. 

#### Finisher:
Start a webserver using pods, it shall have persistant storage, start at boot in a non root repository. It shall use the port 7070. 