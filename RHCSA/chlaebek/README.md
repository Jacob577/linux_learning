### From [chlebik](https://github.com/chlebik/rhcsa-practice-questions/tree/master/questions) 

1. Create new network connection using provided values: IP: some.ip.to.be.used/mask Gateway: some.gateway.to.be.used DNS server: some.dns.to.be.used Interface: eth0 
<br>
2. Install Apache and allow it to get documents from NFS mounted folder (SELinux part) on /nfs
```bash
semanage boolean -l | grep -ni nfs | grep -ni httpd
# httpd_use_nfs

semanage boolean -m httpd_use_nfs --on

# Check what context root dir has originally
ls -Z /var/www/html
# httpd_sys_content_t
semanage fcontext -a -t httpd_sys_content_t "/nfs(/.*)?"
restorecon -R -v /nfs
```
<br>
3. Add label to an existing LV
```bash
umount <path/to/mount/point>
# We can either use lvmrename or xfs_admin
lvrename /dev/VG/old_name /dev/VG/new_name
# or
lvrename VG-name old_name new_name

xfs_admin -L myFS <path>

mount -a
```
<br>
4. Assign the same SELinux contexts used by the home directories to the /xfs directory permanently.
```bash
# Check what context the home folders have
ls -Z /home
# user_home_dit_t

semanage fcontext -a -t user_home_dit_t "/xfs(/.*)?"
restorecon -R -v /xfs
```
<br>
5. Allow davis (and only davis) to get full access to john's home directory.
<br>

6. Set the default target to boot into X Window level (previously level 5).
```bash
man runlevel
systemctl set-default graphical.target
```
<br>
7. Add additional repository for YUM with name my_custom_repo which can be found via URL http://local.repo/rhel7
<br>
8. Reduce the size of existing logical volume by 400MB.
```bash
umount <mount/point>
lvreduce -L -400M -r <path>
mount -a
```
<br>
9. Configure journald to persist between reboots
```bash
vi /etc/systemd/journal.conf
# Enable SystemMaxUse and set to 500M and modify Storage=auto --> persistent
systemctl deamon-reload
```
<br>
10. Create pool and filesystem for thin provisioning and snapshots.
```bash
lvcreate -s --thinpool vg001/pool origin_volume --name mythinsnap
```
<br>
11. Login to the registry. Download image of a web server. Run the web server in a container as a user-service on port 8080, sharing files from /home/user/webfiles. Ensure the web server is available across reboots. Add persistant storage.
<br>
12. Find All Files in /etc (not subdirectories) that contain text "chrony" (ignore case).
[Source](https://stackoverflow.com/questions/16956810/how-to-find-all-files-containing-specific-text-string-on-linux)
```bash
# To get all files that has chrony in the name
grep -lis chrony /etc/*

# However to find all files that contain text:
grep -rnw /etc -e chrony

```
<br>
13. Find All Files in /etc (not subdirectories) that where modified more than 180 days ago.
```bash
find /etc -type f -maxdepth 1 -mtime +180

# To find these alternatives, search for depth, since we don't want to go further than basedir, then time. 
```
<br>
15. Search the string sarah in the /etc/passwd file and save the output in /root/lines
```bash
cat /etc/passwd | grep (-ni) > /root/lines
```
<br>
16. Bind with LDAP located on ldap.server.com

    LDAP dn is dc=server,dc=com
    certificate file is located on http://ldap.server.com/pub/EXAMPLE-CA-CERT
    ldapuserX should be able to log into your system, where X is server domain number but will have none home directory (configured in different question)
    all LDAP users have password password

Note:

According to CertDepot, Objectives around Virtualization and LDAP configuration are gone. Additionally, authconfig-gtk is no longer available for RHEL8. Instead, authselect should be used to configure the system to use SSSD, and SSSD configured to use LDAP, if LDAP authentication is required.
<br>
17. Configure autofs to automount the home directories of LDAP users.

    classroom.example.com (172.25.254.254) NFS-exports /home/guests to your system, whereX is Your server number
    LDAP userX home directory is classroom.example.com:/home/guests/ldapuserX
    ldapuserX home directory should be automounted locally beneath /home as /home/guests/ldapuserX
    home directories must be writable by their users
    while You are able to login as any of the users ldapuser1-20 the only home directory You are able to access is ldapuserX
<br>
18. Create a new physical volume with volume group in the name of datacontainer, the extent of VG should be 16MB. Also create new logical volume with name datacopy with the size of 50 extents and filesystem vfat mounted under /datasource.

19. Create an NFS server and connect a client with autofs
```bash
# Create an exports file
mkdir /share

# On server
chmod -R 777 /share

vi /etc/exports
/share *(rw,sync,no_root_squash)
exportfs



```


### Finisher, start two servers, one NFS host, and one client. On the client server, mount the NFS using autoFS on all home directories