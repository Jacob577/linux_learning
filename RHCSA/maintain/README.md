## Deploy, configure, and maintain systems

- Schedule tasks using at and cron
- Start and stop services and configure services to start automatically at boot
- Configure systems to boot into a specific target automatically
- Configure time service clients
- Install and update software packages from Red Hat Network, a remote repository, or from the local file system
- Modify the system bootloader

### Schedule tasks using at and cron
How would we go about schedueling a task using cron? 
First thing to know is when a crontab should be executed, the convetion is:
`minute, hour, day of month, month, day of week username, <command>`
Whereas if we'd like to specify a range when a crontab to be executed, we can use dash. `1-5` for example.

So if we'd like to have a crontab that backups the journal each hour at 30 min past every weekday:
```bash
# Script at /
#!/bin/bash

30 * * * 1-5 root ./backup_logs.sh 
```


### Start and stop services and configure services to start automatically at boot
To just start and enable services to start on boot, we can run the command `systemctl enable --now <service name>`.
Otherwise, we will have to create a unit file and add to systemd... 
### Configure systems to boot into a specific target automatically
To configure your system to automatically boot into specific targets, you can use the command: `systemctl set-default <runlevel>`, to see what runlevels are available, see the manual for `runlevel`.
### Configure time service clients

### Install and update software packages from Red Hat Network, a remote repository, or from the local file system
To mount a local repository or use a specific:
Local repo:
```bash
mount -o loop /dev/srv0 /repo
cp -rv /repo/media.repo /etc/yum.repos.d/rhel.repo

# You can get assistance with the naming scheme with the help of the /repo, there should be one applications and one baseOS

# Modify the rhel.repo so that it looks something like this:
[AppStream]
name=AppStream
gpgcheck=0
enabled=1
baseurl=file:///repo/BaseOS

[BaseOS]
name=BaseOS
gpgcheck=0
enabled=1
baseurl=file:///repo/BaseOS

# Save and everything should now be fine! You can exchange the file:... with a url if you'd like to use an online repository
```
### Modify the system bootloader
When modifying the system bootloader, we modify `/etc/default/grub`, and then we run `grub2-mkconfig /etc/default/grub > /boot/grub2/grub.cfg`
