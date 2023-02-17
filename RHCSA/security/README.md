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
### Set enforcing and permissive modes for SELinux
To set enforcing or permissive permissions in SELinux, we'll have to modify `/etc/selinux/config`. Thereafter it is not possible to "activate" it without rebooting the machine. 

It is described what parameters to modify in the file. RTFM.


### List and identify SELinux file and process context
### Restore default file contexts
### Manage SELinux port labels
### Use boolean settings to modify system SELinux settings
### Diagnose and address routine SELinux policy violations
A regular SELinux policy violation is if a nonstandard port is used for example an Apache service. 