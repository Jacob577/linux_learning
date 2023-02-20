## Manage security

- Configure firewall settings using firewall-cmd/firewalld
- Manage default file permissions
- Configure key-based authentication for SSH
- Set enforcing and permissive modes for SELinux
- List and identify SELinux file and process context
- Restore default file contexts
- Manage SELinux port labels
- Use boolean settings to modify system SELinux settings
- Diagnose and address routine SELinux policy violations

### Configure firewall settings using firewall-cmd/firewalld
### Manage default file permissions

### Configure key-based authentication for SSH
To configure key-based login via SSH, we first need to create a key. It is important to keep the key private, especially if the key is valid for multiple servers and allows for Ansible playbooks to be run on multiple machines.
```bash
ssh-keygen
ssh-copy-id <username>@<IP of server>

# Hardening the system further:
vi /etc/ssh/sshd_config

[...]
PasswordAuthetication no
[...]

# In /etc/ssh/sshd_config there is also a good description of how to 
# change selinux to a nonstandard port
```
### Set enforcing and permissive modes for SELinux
To set enforcing or permissive permissions in SELinux, we'll have to modify `/etc/selinux/config`. Thereafter it is not possible to "activate" it without rebooting the machine. 

It is described what parameters to modify in the file. RTFM.

### List and identify SELinux file and process context
To list the SELinux contexts for a current directory, we can use the command: `ls -Z`. 


### Restore default file contexts
To restore SELinux file contexts: use the command `restorecon`. I like to use `-v` flag to make the changes vevrbose.

Example:
```bash
# An example:
restorecon -vR /var/www/html
```

To set secontexts, we can use the commands: 
```bash
# After finding the right context to add, with:
semanage fcontext --list | grep -ni <whatever>

# As for usual -R, recursive is for applying changes 
# to subdirectories and -t for type
chcon -R -t httpd_sys_content_t <directory-name>

# A way of applying context
semanage fcontext -a -t user_home_dir_t "/xfs(/.*)?"
restorecon -R /xfs
```
### Manage SELinux port labels
How would you go about starting an Apache service with a non-standard port, 7070 securely? 
```bash
rpm -qa | grep httpd
dnf install httpd -y

# Edit port:
vi /etc/httpd/config
[...]
Listening 7070
[...]
systemctl enable --now httpd

# Edit the /var/www/html/index.html as you'd like
firewall-cmd --add-service=httpd
firewall-cmd --add-port=22/tcp --permanent
semanage port -a -t http_port_t -p tcp 7070

systemctl restart httpd
```
### Use boolean settings to modify system SELinux settings
Credit to [RedHat](https://www.redhat.com/sysadmin/change-selinux-settings-boolean)

One common boolean is to allow Nginx or Apache to access home directories or other directories other than `/var/www/html`. 

An example command for allowing httpd to access home dirs:
```bash
semanage boolean -m --on httpd_enable_homedirs
```

Another way of setting a sebool is:
```bash
setsebool -P httpd_enable_homedirs 1 

# -P flag is important to make the change persistant
```
### Diagnose and address routine SELinux policy violations
A regular SELinux policy violation is if a nonstandard port is used for example an Apache service. 

### Extra
To run a script at boot, we can use crontab with the decorator @reboot

```bash
crontab -u <user> -e
@reboot bash /<path>
```