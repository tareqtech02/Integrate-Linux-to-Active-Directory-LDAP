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

Change this Line `ONBOOT=no` to `ONBOOT=yes`


# Install LDAP on Centos

Set the new hostname using hostnamectl
```
hostnamectl set-hostname CentOS8
```

Display repository list
```
yum repolist
```

Update your system
```
yum update -y
```

If you have same errro in CentOS 8 Report Please run this commands 
```
sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*
```

Install the System Security Services Daemon (SSSD)
Install realmd, a tool for configuring authentication and domain membership
Install oddjob, a helper service for managing a user's home directory
Install adcli, a command-line tool for Active Directory integration
Install Samba common files
Install OpenLDAP client utilities
```
yum install -y sssd realmd oddjob oddjob-mkhomedir adcli samba-common samba-common-tools krb5-workstation openldap-clients policycoreutils-python-utils.noarch
```


Edit the host file to include DNS mappings
```
vim /etc/hosts

```
Add Your domain IP and Hostname 
```
192.168.1.100  dc tareqtech.local dc.tareqtech.local
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

Create This file in this Path
```
vim /etc/NetworkManager/conf.d/dns-none.conf
```
Add This content inside the file 
```
[main]
dns=none
```

You Can  discover the domain 
```
realm discover tareqtech.local
```


Join the system to the Active Directory domain
```
realm join --user=tareq tareqtech.local
```


Display information about the realm and domain
```
realm list
```

Get some info of AD Users
```
getent passwd administrator@tareqtech.local
```


Leave the Active Directory domain
```
realm leave
```

Remove Fully qualified domain name (FQDN)
```
vim /etc/sssd/sssd.conf
```
Edit `fallback_homedir = /home/%u@%d` to `fallback_homedir = /home/%u`  
Edit `use_fully_qualified_names = True` to `use_fully_qualified_names = False`  

Restart Services System Security Services Daemon (SSSD)
```
systemctl restart sssd
```

Edit sudoers file to allow wheel group members to run commands without a password
```
visudo
%sysadmin        ALL=(ALL)       NOPASSWD: ALL
```
## faillock utility to enforce account lockouts for local users
Add these lines in system-auth and password-auth files 
```
vim /etc/pam.d/system-auth
vim /etc/pam.d/password-auth
```
Add 3 lines to the auth section 
```
auth        required                                     pam_faillock.so preauth silent audit deny=2 unlock_time=600
auth        sufficient                                   pam_unix.so nullok try_first_pass
auth        [default=die]                                pam_faillock.so authfail audit deny=2 unlock_time=600
```
Add 1 line to the account section
```
account     required                                     pam_faillock.so
```
