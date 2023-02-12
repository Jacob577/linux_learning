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

So if we'd like to have a crontab that backups the journal each minute:
```bash
# Script at /
#!/bin/bash


```


### Start and stop services and configure services to start automatically at boot
To just start and enable services to start on boot, we can run the command `systemctl enable --now <service name>`.
Otherwise, we will have to create a unit file and add to systemd... 
### Configure systems to boot into a specific target automatically
### Configure time service clients
### Install and update software packages from Red Hat Network, a remote repository, or from the local file system
### Modify the system bootloader
