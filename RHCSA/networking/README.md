## Manage basic networking

- Configure IPv4 and IPv6 addresses
- Configure hostname resolution
- Configure network services to start automatically at boot
- Restrict network access using firewall-cmd/firewall

### Configure IPv4 and IPv6 addresses
Configuring IPv4 & 6 can easily be configured with the command `nmtui`, it is a semi-graphical user interface with a great selection of options and very intuiative. 

Whenever a change has been made in the networking, `systemctl networking off/on` should be considered. Similar to whenever a firewall setting has been changed, the firewall has to be reloaded.
### Configure hostname resolution
Whenever you'd like to configure the hostname resolution, you would use: `systemctl set-hostname` and then restart the machine. 
Enable service: `systemctl enable --now <service>`

We could also use `nmcli networking off/on`
### Configure network services to start automatically at boot
Credit [cengland](https://www.youtube.com/watch?v=UPYiSRGRVlk) 
To add a service to start at boot, we can go the same route as for automatically starting any other service; use `systemctl enable <service> --now`.
To reload network, we can use: `systemctl restart NetworkManager`.

In other terms, we can use the command:
`nmcli connection modify eth0 connection.autoconnect yes`
With this command we enable a network interface to start at boot. 


### Restrict network access using firewall-cmd/firewall
Important to remember whenever doing changes in the firewall, add `--permanent` flag to persistantly change the configuration in the firewall. After changes we need to use the `--reload` flag.

Credit to [cengland](https://www.google.com/search?client=firefox-b-d&q=Restrict+network+access+using+firewall-cmd%2Ffirewall+#fpstate=ive&vld=cid:ca7895bf,vid:ZNmTKAdDnrc)

To list zones:
`firewall-cmd --get-zones`

To list all entries in a zone, we can add `--list-all`, ei.
`firewall-cmd --zone <zone name> --list-all`

Create new zone:
`firewall-cmd --permanent --new-zone <zone name>`

We might want to add services to specific zones:
`firewall-cmd --zone <zone name> --add-service=<service> --permanent`

After adding service, we usually want to add an interface:
`firewall-cmd --change-interface=enp0s3 --zone <zone name> --permanent` for exampl. 

How to list all of the interfaces:
`firewall-cmd --get-active-zones`
Set default zone: `firewall-cmd --set-default-zone=<zone name>`

Whitelist services:
Search for service: `firewall-cmd --get-services | grep <service>`
add service to specific zone: `firewall-cmd --add-service=<service> --permanent --zone <zone name>`
Add port:
`firewall-cmd --add-port=8080/tcp --permanent`