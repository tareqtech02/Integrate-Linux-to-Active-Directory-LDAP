# Integrate-Linux-to-Active-Directory-LDAP


In CentOS

Edit sshd file to let root login via ssh session 
```
vim /etc/ssh/sshd_config
```
Change from 'PermitRootLogin yes' to 'PermitRootLogin yes'


Check Connection name
```
nmcli connection show
```

Activate the network connection using NetworkManager command-line interface
```
nmcli connection up enp0s3
```

Add User to login via SSH
```
useradd tareq
passwd tareq
```

To Allow root to access via ssh 
```
sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/g' /etc/ssh/sshd_config
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
