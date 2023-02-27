1. create a VG of 2GB and call it vgprac
```bash

```
2. Use the appropriate utility to create a 5TiB thin provisioned volume
```bash

```
3. Enable packet forwarding on system1. This should persist after reboot
```bash

```

4. Write a script named awesome.sh in the root directory on system1.
if "me" is given as an argument, then the script should output "yes, I'm awesome"
if "them" is given as an argument, then the script should output: "Okay, they are awesome"
if the argument is empty or invalid, return "Usage ./awesome.sh me|them"
```bash
#!/bin/bash

if [ "$1" = "me" ] ; then
echo "I am awesome"

elif [ $1 == "them" ] ; then 
echo "They are awesome"

else
echo "Usage ./awesome.sh me|them"
fi

```
5. Find all files > 5M and copy them to /find/large
```bash
mkdir -p /find/largefiles
find / -size +5M -exec cp {} /find/largefiles \;
```

6. Create a cron job that writes "This practice exam was easy and I'm ready to ace my RHCSA" to /var/log/messages at 12pm only on weekdays
```bash
crontab -u root -e
0   12  *   *   1-5 root echo "This pracice exam was easy and I'm ready to ace my RHCSA" >> /var/log/messages
```
## Learned
when resizing lvs, we can use the `-r` flag so that we resize the filesystem as well. 

practice more bash scripting
