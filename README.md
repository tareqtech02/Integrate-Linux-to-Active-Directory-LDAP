# Integrate-Linux-to-Active-Directory-LDAP


Fix Some Issue in CentOS



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

Restart SSHD Services 
```
systemctl restart sshd
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


# Install LDAP on Centos

Set the new hostname using hostnamectl
```
hostnamectl set-hostname CentOS9
```

Display repository list
```
yum repolist
```

Update your system
```
yum update -y
```

Install the System Security Services Daemon (SSSD)
```
yum install sssd -y
```


Install realmd, a tool for configuring authentication and domain membership
```
yum install realmd -y
```

Install oddjob, a helper service for managing a user's home directory
```
yum install oddjob -y
```


Install oddjob-mkhomedir, a component for automatically creating home directories
```
yum install oddjob-mkhomedir -y
```

Install adcli, a command-line tool for Active Directory integration
```
yum install adcli -y
```


Install Samba common files
```
yum install samba-common -y
```


Install Samba common tools
```
yum install samba-common-tools -y
```


Install Kerberos client packages
```
yum install krb5-workstation -y
```

Install OpenLDAP client utilities
```
yum install openldap-clients -y
```


Install SELinux utilities for Python
```
yum install policycoreutils-python-utils.noarch -y
```

Edit the host file to include DNS mappings
```
vim /etc/hosts

```
Add Your domain IP and Hostname 
```
192.168.1.100  win_ser01 tareqtech.local win_ser01.tareqtech.local
```


Edit the resolv.conf file for DNS configuration
```
vim /etc/resolv.conf
```
Add This contents
```
search tareqtech.local
nameserver 192.168.1.100
```

Join the system to the Active Directory domain
```
realm join --user=administrator tareqtech.local
```


Display information about the realm and domain
```
realm list
```


Leave the Active Directory domain
```
realm leave
```


Edit sudoers file to allow wheel group members to run commands without a password
```
visudo
%wheel        ALL=(ALL)       NOPASSWD: ALL
```

