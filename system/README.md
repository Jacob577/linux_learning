## Operate running systems
A systems administrator need to know his/her system and how to configure it to his/her own or companies preferences. 

- Boot, reboot, and shut down a system normally
- Boot systems into different targets manually
- Interrupt the boot process in order to gain access to a system
- Identify CPU/memory intensive processes and kill processes
- Adjust process scheduling
- Manage tuning profiles
- Locate and interpret system log files and journals
- Preserve system journals
- Start, stop, and check the status of network services
- Securely transfer files between systems

### Boot, reboot, and shut down a system normally
```bash
# Shut down
init 0

# Reboot
init 6
```

### Boot systems into different targets manually
To list the available runlevels if you were to forget, you can use `man runlevel`. Thereafter run `init <runlevel>`. This will temporarly boot the system into other runlevels. To make it permanent, use `systemctl set-default <runlevel>`

### Interrupt the boot process in order to gain access to a system
Let's say you've forgotten the root password, what do you do?
1. Boot the machine and escape the boot process by pressing `e`.
2. Break the boot process by adding `rd.break`.
3. Continue into grub by pressing `ctrl+x`
4. When in grub terminal, run:
```bash
mount -o remount,rw /sysroot
chroot /sysroot
passwd root
<enter new password>
sync
touch /.autorelabel
exit
exit
```
5. You should now be able to enter your system with the new credentials 

### Identify CPU/memory intensive processes and kill processes
To identify demanding processes we can look in top, by default, it is sorted by CPU capacity, you can sort by memory with `m`. To kill demanding process use `kill <PID>`. 

### Adjust process scheduling
When adjusting process schedueling, we use the commands `nice` and `renice`. `renice` is used whenever we want to rescheduele a task priority while `nice` is used to just scheduele. The scale reaches from -20 to 20 where 20 is the highest and 0 is default. 

To check nice-values, we can run `ps -l`. We can thereafter grep for specific tasks. 

Syntax is as follows:
`nice -<priority> <command-argument>`

We can renice by PID:
`renice -n <priority> -p <PID>`

and to renice entire groups:
`renice -n <priority> -g -p <PID>`

We can also adjust all processes by a user:
`renice -n <priority> -u johndoe`

### Manage tuning profiles
`tuned-adm` is quite a straight forward command with good documentation. 
```bash
tuned-adm list
tuned-adm active
tuned-adm profile <profile>
tuned-adm recommend
```

### Locate and interpret system log files and journals
[Recommended read (RedHat)](https://www.redhat.com/sysadmin/rsyslog-systemd-journald-linux-logs)

Using `journalctl`
To view the last entries of the journal we can use:
`journalctl -n <number of lines>`

You can trail journal by UID using:
`journalctl _UID=<UID>`

Journal entries based on process:
`journalctl -u <process>` e.i. sshd

We can check the logs based on time with:
`journalctl -u httpd -since "1 hour ago"`

Individual logs are located in `/var/logs/` here we can also access `messages` and `secure`. 

### Preserve system journals
The [RedHat textbook way](https://www.redhat.com/sysadmin/store-linux-system-journals) is as follows for preserving journals.

```bash
mkdir /var/log/journal
vi /etc/systemd/journald.conf

[...]
[Journal]
Storage=persistent
[...]

systemctl restart systemd-journal
```

Thereafter we can limit the allowable space for the journals by modifying:
`/etc/systemd/journal.conf` and set the max use. Thereafter restart the service using: `systemctl restart systemd-journald`. 

### Start, stop, and check the status of network services
### Securely transfer files between systems