# Practice exams
Many of these questions are fetched from other sources, I have not come up with them, credit goes to:

A great YouTube channel!
[Computers, Security & Gadgets](https://www.youtube.com/@compsecgadgets)

Great book, just great.
[Sander Van Vugt - RHCSA cert guide](https://digtvbg.com/files/LINUX/Vugt%20S.%20-%20Red%20Hat%20RHCSA%208%20Cert%20Guide%20-%20EX200%20%28Certification%20Guide%29%20-%202020.pdf)

A guy who published a lot of questions related to the RHCSA exam 
[cgkebik](https://github.com/chlebik/rhcsa-practice-questions/tree/master/questions)

## The first set of questions are from [Sanders](https://digtvbg.com/files/LINUX/Vugt%20S.%20-%20Red%20Hat%20RHCSA%208%20Cert%20Guide%20-%20EX200%20%28Certification%20Guide%29%20-%202020.pdf)

Before we begin, I know I have a few weaknesses that I know I have to work on. I find it difficult with `autoFS`. I am poor with `containers`, I need to practice more on `NTP`. 

1.  Configure your system to automatically loop-mount the ISO of the
installation disk on the directory /repo. Configure your system to remove
this loop-mounted ISO as the only repository that is used for installation.
Do not register your system with subscription-manager, and remove all
reference to external repositories that may already exist.
<br>
2. Reboot your server. Assume that you don’t know the root password, and
use the appropriate mode to enter a root shell that doesn’t require a
password. Set the root password to mypassword.
<br>
3. Set default values for new users. Set the default password validity to
90 days, and set the first UID that is used for new users to 2000.
<br>
4. Create a 2-GiB volume group, using 8-MiB physical extents. In this volume
group, create a 500-MiB logical volume with the name mydata, and mount
it persistently on the directory /mydata.
<br>
5. Find all files that are owned by user edwin and copy them to the directory
/rootedwinfiles.
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

### From [chlebik](https://github.com/chlebik/rhcsa-practice-questions/tree/master/questions) 

1. Create new network connection using provided values: IP: some.ip.to.be.used/mask Gateway: some.gateway.to.be.used DNS server: some.dns.to.be.used Interface: eth0 
<br>
2. Install Apache and allow it to get documents from NFS mounted folder (SELinux part)
<br>
3. Add label to an existing LV
<br>
4. Assign the same SELinux contexts used by the home directories to the /xfs directory permanently.
<br>
5. Allow davis (and only davis) to get full access to john's home directory.
<br>
6. Set the default target to boot into X Window level (previously level 5).
<br>
7. Add additional repository for YUM with name my_custom_repo which can be found via URL http://local.repo/rhel7
<br>
8. Reduce the size of existing logical volume by 400MB.
<br>
9. Configure journald to persist between reboots
<br>
10. Create pool and filesystem for thin provisioning and snapshots.
<br>
11. Login to the registry. Download image of a web server. Run the web server in a container as a user-service on port 8080, sharing files from /home/user/webfiles. Ensure the web server is available across reboots.
<br>
12. Find All Files in /etc (not subdirectories) that contain text "chrony" (ignore case).
<br>
13. Find All Files in /etc (not subdirectories) that where modified more than 180 days ago.
<br>
14. Search the string sarah in the /etc/passwd file and save the output in /root/lines
<br>
15. Search the string sarah in the /etc/passwd file and save the output in /root/lines
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

### Finisher, start two servers, one NFS host, and one client. On the client server, mount the NFS using autoFS on all home directories

## What I need to practice:
1. autofs on all home directories
2. bash scripting
3. how to error search with journalctl
4. more advanced `sed` combinations and sorting
5. containers