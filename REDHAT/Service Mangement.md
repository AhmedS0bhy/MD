# Configure a Bind DNS
## Install BIND DNS 
```sh
yum install bind bind-utils  # for installing the services 
systemctl named # the name of the service 
```
## Configuration file 
```sh
/etc/named.conf
```
### File content
```sh
listen-on port 53 {any; <specify interface ip>};         # Spicify the listen port 
listen-on-v6 port 53 {none;};                            # spicify the listen v6 
allow-query {localhost; 192.168.56.0/24; localnets;};    # allowed hosts to query 
forwarders {<forward DNS>};                              # the forwaders DNS 
forward only;                                            # !!!
```
### check the configuration 
```sh
named-checkconf 
```

## log file and zones 
```sh
/var/named
```
### 1. add zone to name.conf 
```sh
zone "example.com." {
    type master;
    file "<name of the zone file>";
    allow-update {none;};
};  
``` 
### 2. zone file 
```sh
$TTL 3H
$ORIGIN example.com.
example.com IN SOA  server1.example.com. root.example.com. (
                    1   ;   serial 
                    1D  ;   refresh
                    1H  ;   retry
                    1W  ;   expire
                    3H );   minimum 

example.com.    NS  server1.example.com.
server1         A   192.168.56.106
```
## Configure the DNS client 
```sh
PEERDNS=yes 
DNS1=<server IP>
```
# Configure FTP 
## Install Servier
```sh
yum install -y vsftpd 
systemctl enable vsftpd 
systemctl start vsftpd 
```
## Configure file 
```sh
/etc/vsftpd/vsftpd.conf
```
### file content 
```sh
anonymous_enalbe=YES
local_enalbe=NO
write_enable=NO
local_unmaks=022
dirmessage_enable=YES
xferlog_enable=YES
connect_from_port_20=YES
xferlog_std_format=YES
listen=YES
listen_ipv6=NO
pam_service_name=vsftpd
userlist_enable=YES
tcp_wrappers=YES
anon_world_readable_only=YES
```
## Create Repo to ftp 
```sh
mkdir /var/ftp/pub/centos72 # make the directory to contain the files in FTP 
mount /dev/sr0 /mnt # mount the DVD
find /mnt/ | cpio -pmd /var/ftp/pub/centos72 # move files to dir 
```
### Create repo 
```sh
/etc/yum.repos.d/<name of repo>.repo
```
#### repo content 
```sh
[FTPc7]                                         # name of repo 
name=FTP CentoOS 7.2                            # comment for repo 
baseutl=ftp://server1.exampl.mv/pub/centos72    # location of the ISO 
enabled=1                                       # to enable the repo 
gpgcheck=0                                      # disable the gpgcheck
```
# Confiure DHCP 
## Configure static Ip 
```sh
BOOTPROTO=none
IPADDR='192.168.56.106'
NETMASK='255.255.255.0'
PERRDNS='YES'
DNS1='127.0.0.1'
```
## Install DHCP
```sh
yum install -y dhcp 
```
## Confiugre DHCP 
### Confifuration File
```sh
/etc/dhcp/dhcpd.conf
```
### Content 
```sh
option domain-name-servers 192.168.56.106;
option domain-search "example.com";
default-lease-time 86400;
ddns-update-style none;
authorative;
log-facility local4;

subnet 192.168.56.0 netmask 255.255.255.0{
    range 192.168.56.151 192.168.56.254;
}

host server2 {
    hardware ethernet <mac address>;
    fixed-address 192.168.56.120;
}
```
## test DHCP 
```sh
dhclinet -r <interface> # relaease the IP
dhclinet <interface>
cat /var/lib/dhclient/dhclient 
```
# Configure Printing 
## Installing the service 
```sh
yum install cups 
```
## configuration file 
```sh
/etc/cups/cupsd.conf
``` 
### chnage file 
```sh
DefaultEncryption Never
Listen 192.168.106:631
<LOcation />
Order allow, deny 
allwo localhost 
allow 192.168.56.0/24
</Location>

<location /admin >
Order allow, deny 
allow localhost 
allow 192.168.56.0/24 
</Location >
```
### access web interface 
```sh
http://<Server ip>:631
```
## Manage from CLI 
```sh
lpinfo -m #list module
lpadmin -p p1 -v paralle1:/dev/lp0 -m <drv uri>
cupsaccept p1 ; cupsenable p1 
lpoptions -d p1 
```

# Configure Apache 
## Install the service 
```sh
yum install httpd mod_ssl httpd-manual 
```
## ServerRoot conf file 
```sh
/etc/httpd/
    - conf/
        - httpd.conf
    - conf.d/
        -manula.conf 
    - conf.modules.d/ 
```
## virtulaHost 
```xml
<VirtualHost *:80>
    ServerName "moodle.example.vm"
    DocumentRoot "/var/www/moodle"
</VirtualHost>
```
### VirtualHost with IP restrication 
```xml
<VirtualHost *:80>
    ServerName "moodle.example.vm"
    DocumentRoot "/var/www/moodle"
    <Directory "var/www.moodle">
        Require ip 192.168.
        Require IP local 
    <Directory>
</VirtualHost>
```
### Create User Password File 
```sh
htpasswf -c /etc/httpd/conf.d/moodle <name>
htpasswf /etc/httpd/conf.d/moodle <name>
# Secnond user we don't need the -c 
```
### VirtualHost with user 

```xml
<Directory "var/www.moodle">
        Require valid-user
        AuthType Basic 
        AuthName "Private Access"
        AuthBasicProvider fiel 
        AuthUserFile "/etc/httpd/conf.d/moodle"
<Directory>
```
## SSL 
### Generate CRT 
```sh
openssl req -new -nodes -x509 -keyout s1.key -out ssl.crt 
```
### VirtualHost Conf with ssl
```xml
<VirtulaHost *:443>
    ServerName "moodle.example.vm"
    DocumentRoot "/var/www/moodle"
    SSLEngine on 
    SSlCertificateKeyFile "conf.d/moodle.key"
    SSLCertificateFile "conf.d/moodle.crt"
```
## Proxy Server
### install squide 
```sh
yum install squid
```
### Configure linux client 
```sh
export http_proxy=http://server1.example.vm:3128
```

# Install and test PHP 
## Install pacakge 
```sh
yum install -y php 
systemctl restart apacher 
```
## Command Line PHPINFO 
```
php -i  # loaded Configuratio ifle 
```
# install MariaDB 
## Install
```
yum install -y mariadb mariadb-server php-mysql 
```
## Secure server 
```sh
myssql_secure_installation 
```
## login 
```sh
myslq -u root -p 
```

# Configure SElinux 
