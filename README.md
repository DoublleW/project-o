## Folder `ansible`
 - It contains a playbook for deploying a kubernetes cluster with 1 master and 3 workers and 1 host nginx.

## Preparing the servers.


# Ubuntu 18.04 LTS

Turn off `cloud-init` as desired
```
echo 'datasource_list: [ None ]' | sudo -s tee /etc/cloud/cloud.cfg.d/90_dpkg.cfg

sudo apt-get purge cloud-init

sudo rm -rf /etc/cloud/; sudo rm -rf /var/lib/cloud/
``` 

 - Network configuration

If `netplan` is not turned off, change
```
$ sudo nano /etc/netplan/50-cloud-init.yaml

# /etc/netplan/50-cloud-init.yaml
network:
    version: 2
    ethernets:
        ens32:
            dhcp4: no
            addresses: [10.1.1.13/24]
            gateway4: 10.1.1.1
            nameservers:
                addresses: [10.1.1.1, 8.8.8.8]


$ sudo netplan apply
```

 - Turn off `netplan`
```
$ sudo apt-get install -y ifupdown

$ sudo rm -rf /etc/netplan/*.yaml

$ sudo nano /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg

# /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg
network:
{
config: disable
}
```
Customize the interface
```
$ sudo nano /etc/network/interfaces

# /etc/network/interfaces
auto lo
iface lo inet loopback

allow-hotplug ens32
auto ens32
iface ens32 inet static
  address 10.1.1.10
  netmask 255.255.255.0
  gateway 10.1.1.1
  dns-nameservers 10.1.1.1 8.8.8.8

$ sudo service networking restart
```
 - Host name

Change hostname when `netplan` is enabled
```
$ sudo hostnamectl set-hostname <hostname>

$ sudo nano /etc/cloud/cloud.cfg

# This will cause the set+update hostname module to not operate (if true)
preserve_hostname: true

$ sudo reboot
```

 - Change hostname when `netplan` is off.

Set fqdn server.
```
$ sudo nano /etc/hostname
```
If there is no DNS server, then we write the host name in `/etc/hosts`
```
$ sudo nano /etc/hosts

127.0.0.1       localhost.localdomain   localhost  
127.0.1.1       <host_name>
```
Disable `systemd-resolved`
```
$ sudo systemctl disable systemd-resolved.service

$ sudo systemctl stop systemd-resolved.service

$ sudo service networking restart
```


# Debian GNU/Linux 9 (stretch)

 - You must add `sudo` rights for the user.
```
$ su

# apt-get update

# apt-get install sudo -y

# usermod -a -G sudo <user>

# reboot
```

 - Customize the interface
```
$ sudo nano /etc/network/interfaces

# /etc/network/interfaces
auto lo
iface lo inet loopback

allow-hotplug ens32
auto ens32
iface ens32 inet static
  address 10.1.1.10
  netmask 255.255.255.0
  gateway 10.1.1.1
  dns-nameservers 10.1.1.1 8.8.8.8

$ sudo service networking restart
```


 - Change hostname.

Set `fqdn` server.
```
$ sudo nano /etc/hostname

<ip_address>   <name_server>
```
If there is no DNS server, then we write the host name in `/etc/hosts`
```
$ sudo nano /etc/hosts

127.0.0.1       localhost.localdomain   localhost  
127.0.1.1       <host_name>
```
Disable `systemd-resolved`
```
$ sudo systemctl disable systemd-resolved.service

$ sudo systemctl stop systemd-resolved.service

$ sudo service networking restart
```

 - Error
```
ping: google.com: Temporary failure in name resolution
```
Change recording
```
sudo nano /etc/resolv.conf

nameserver 8.8.8.8
```


## Preinstall and build the kubernetes cluster.

Go to the `ansible` folder, change the parameters and start the playbook.
