# Integrate-Linux-to-Active-Directory-LDAP


In CentOS


Check Connection name
```
nmcli connection show
```

Activate the network connection using NetworkManager command-line interface
```
nmcli connection up enp0s3
```


Edit the configuration file for the enp0s3 network interface
```
vim /etc/sysconfig/network-scripts/ifcfg-enp0s3
```
Add This content
```
DEVICE="enp0s3"
ONBOOT=yes
NETBOOT=yes
BOOTPROTO=dhcp
TYPE=Ethernet
NAME="enp0s3"
```
