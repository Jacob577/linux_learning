# Recap

## Zipping and unzipping + archiving
```bash
# gzip & bzip2:
gzip <file>
gzip -d <file.gz>


# To tar a file: 
tar cvf <compressed_name.tar> <file_to_compress>

# To tar and gzip (j instead of z to use bzip)
tar cvzf <compressed_name.tar.gz> <file_to_compress>

# list tar files:
tar -tf <compressed_file.tar.(gz)>

# Extract (add z for uncompressing gzip)
tar xvf <compressed_file.tar>

# If directory is locked by selinux: (can be recursive)
star -c -f=<filename.star> <file_name_to_compress>
```

### SED
```bash
# replace Kenny w. Lenny
sed -i (insert) 's(substitute)/Kenny/Lenny/g' file.txt

# Find and delete every line that contains a word
sed '/word/d' file.txt

# Remove empty lines
sed '/^$/d' file.txt

# Remove the first line 
sed '1d' file.txt
sed '1,2d' file.txt

# Replace tab with space
sed 's/\t/ /g' 

# I want to replace every sinefield, except line 8
sed '8!s/Sinefield/S/g' file.txt

# Replace in vim:
:%s/<word to replace>/<replace with>
```

### Selinux
```bash
# to restore default when dabbled too much with the contexts
restorecon <file> (-R for recursive) <path>

# To set additional booleans for selinux
semanage boolean -m --on use_virtualbox

# check if it's on or off
semanage boolean -l | grep virtualbox 

# Diagnose:
sealert -a /var/log/audit/audit.log
```

### Autofs
```bash
# How to mount on users home directories

# On host:
dnf groupinstall -y file-server
firewall-cmd --permanent --add-service=nfs
firewall-cmd --permanent --add-port=111/tcp
firewall-cmd --reload

systemctl enable --now rpcbind nfs-server

mkdir /home/nfs-share
chmod 777 /home/nfs-share

dnf install -y setroubleshoot-server

setsebool -P nfs_export_all_rw on
setsebool -P nfs_export_all_ro on
setsebool -P use_nfs_home_dirs on

# Edit the /etc/exports file and create a new line with the following:
/home/nfs-share <IP>(rw,no_root_squash)

systemctl restart nfs-server
showmount -e localhost

# On client
---



```

### Run containers in a non-root configuration

```bash
# Run pod as a non-root
podman run --user 200 -it -v $(pwd)/myfolder:/mnt/myfolder:Z busybox
```