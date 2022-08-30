# Introduction

## ip vs ifconfig 

```sh
ifconifg ens33 # Display interface configuration 
```
## using ip command 
```sh
ip addr         # get iP address 
ip r            # route 
ip n            # neghbors
ip netns        # name space 
```
## Create network name space 
```
sudo ip netns add <network space name>
```
# Configuring hostnames

## viewing the hostname 
```sh
hostname        # print the host name 
hostname -f     # FQN
uname -n        # get host name 
hostnamectl     # get host name  
```
## change hostname 
```sh
hostname <hostname>                     # not perissistent
vi etc/hostname                         # hostname file 
hostnamectl set-hostname <hostname>     # set host name using hostnamectl
hostnamectl set-hostname '<hostname>'   # set pretty name 
``` 
## host name resolve 
```sh
vi /etc/hosts # host resolve file `ex: IP FQDN Alias`
```

## multi cast DNS <===@@
```sh
yum info avahi # get information about the package 
cat /etc/resolv.conf # DNS resolve file
```

# configuring NTP

## linux time 
- system time
- hardware clock 
- hwclock
```sh 
date  # get time and date
date --set="20160406 12:03" # set the time and date to 2016-04-06
hwclock # get the hardware clock 
hwclock --systohc # set time form sys to hc 
timedatectl 
``` 
## implementing chronyd
```sh 
yum install crony # install
vim /etc/crony.conf # the conf file 
chronyc tracking # get the sync time
chronyc sources # get the sync servers 
```
### implemnt ntpd 
```sh
/etc/ntpd.conf 
```

# Configuring Services 
## Display IP 
```sh
ip add show             # show the current IP address
ip -4 a                 # get the ipv4 address 
ip -4 a s <interface>   # get specific interface
```
## assign ip address
```sh
ip addr add <ip addr>/<mask> <interface>
```
## Working with Network manager 
```sh
systemctl status NetworkManager # get the service status 
nmcli connection show           # list our current connections
nmcli -p connection show <connection>       # pretty format 
nmcli connection add con-name <name> ifname <interface name> type <type:ethernet> ip4 <ip addr> gw4 <gateway 4 ipv4> 
nmcli connection down <connection name or iface>
```
## Static interface files 
```sh
cd /etc/sysconfig/network-scripts # is the dir for the connection interface
```
```sh
TYPE # the type of connection - Ethernet 
BOOTPROTO   
IPADDR
NETMASK
DEFROUTE
PEERDNS
NAME
ONBOOT
DNS1
DNS2
```
# Routing 
## routing initial configuration 
### Configure the client 
```sh
ip route add default via <default route> # not perssistence 
vim /etc/sysconfig/network-scripts/<interfacc>
DEFROUTES="YES"
GATEWAY="<Gateway>"
```
### Configure the Server 
```sh
cat /proc/sys/net/ipv4/ip_forward # knwo the ip froward 
vim /etc/sysctl.conf # pressistance conf for IP forward 
insert "net.ipv4.ip_forward=1"
systecl -p # read the configuration 
```

## NAT Firewall   
```sh
iptables -L # list the rule of iptables
iptables -t nat -A POSTROUTING -o <outbound interface> -j MASQUERADE # NAt using iptable
```

# Firewalld 

## basic configuration 
```sh
firewall-cmd --state    # check the firewall state 
systemctl start firewalld  # start the firewalld service
firewall-cmd --get-default-zone # get the default zone 
firewall-cmd --get-active-zones # get all the active zone 
firewall-cmd --get-zones  # get all zones 
firewall-cmd --set-default-zone=<zone> # set a default zone 
```
## zones 
```sh
firewall-cmd --permanent --zone=public --remove-interface={enp0s3,en0s8} # remove interface from zone 
firewall-cmd --permanent --zone=internal --interface=enp0s8 # add interface to zone
```
## add or remove services 
```sh
firewall-cmd --permanent --remove-service=ssh # remove service 
firewall-cmd --permanet --add-sercie=http # add sercie 
firewall-cmd --permanent --new-sercie=puppet # create a new services
vim /etc/firewalld/services/puppet.xml # craete service xml file 
chmod 640 puppet.xml # check service 
``` 
## add NAT
```sh 
firewall-cmd --permanent --add-masquerade --zone=external 

```

# Iptables 
## basic command 
```sh 
iptables -L # list configuration 
iptable -nvL # long list configuration 
iptables -F # flush or remove all the configuration 
iptable-save # save configuration file 
iptables-restore #  restore saved configuration 
```
## Adding rule
```sh
iptables -A INPUT -p tcp --dport 22 -j ACCEPT S
```
## Services 
```sh
yum install iptables-services 
/etc/sysconfig/iptables
/etc/sysconfig/iptables-conf 
```
# Tunnel traffic
## SSH tunnels 
```ssh
ssh -f -L 8080:localhost:80 root@server2 -N
```
## installing openVPN server 
```sh
yum install epel-release #install openvp packages 
yum install openvpn easy-rsa # install openvpn 
iptable -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE # enalbe NAT 
```

## configurating openVPN server
```sh
cp /usr/share/doc/openvpn-2.3.10/sample/sample-config-files/server.conf # copy a server template 
``` 
### configuration 
```sh
/dh # the certficate that server will use 
/redirect # confifure the default gateway 
/DNS # the DNS for the clients 
/user # user that used to login to demon nobody is good 
```
### Create the certifecate 
```sh 
mkdiir -p /etc/openvpn/easy-rsa/key # craete a folder to store the scripts
cp -rf /usr/share/easy-rsa/2.0/* /etc/openvpn/easy-rsa # copy scripts to the folder 
```
#### configure vars 
```sh 
vi /etc/openvpn/easy-rsa/vars )
```
```sh
expert KEY_NAME="<keyname>" 
```
#### make opensll the default 
```sh
cd /etc/openvpn/easy-rsa/
cp openssl-1.0.0.cnf openssl.cnf 
source ./vars 
```
#### run scripts
```sh
./clean-all 
./build-ca 
./build-key-server server 
./build-dh 
```
> all the key will be in key folder 
#### copy the keys to openvpn 
```sh
cp dh2048.pem ca.crt server.crt server.key /etc/openvpn 
```
## Configuring openVPN client
### Generating client config 

```sh
# on server 
cd /etc/openvpn/easy-rsa
./build-key client 
```
> all keys in key folder 
```sh
cp client.key client.crt /etc/openvp 
```
### enable servie 
```sh
systemctl enable openvpn@<servername>
systemctl start openvpn@<servername>
```
### copy the certificate to the client 
```sh
ca.crt
client.crt 
```
#### config temp 
```sh
/usr/share/doc/openvpn-2.3.10/sample/sample-config-files/client.conf  

```
