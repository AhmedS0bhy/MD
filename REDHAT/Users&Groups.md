# introduction 
## listing users and groups 
```sh
cat /etc/passwd  
```
# managing local users 
## Create local user
```sh
useradd <user_name>
useradd -N -g users -G admin <username> # -N no default group -g the primary group -G the secondary group
useradd -s <shell> # spcify diffrent shell
 
username:password:uid:gid:comment:userhome:defaultshell # /etc/passwd columns

```
## manage user password 
```sh
passwd <username>
echo "user2:password1" | chpasswd

chage -l <user> # password attribute and expires 

chage -M <max days> <username> # change the max number to password 
```
## working with users defaults
```sh
less /etc/login.defs
cat /etc/default/useradd 
```
## modufy and delete 
```sh
usermod -c "Comment" <user>
usermod -s /bin/zsh sobhy
userdel -r <user> # remove the home ,cron job
```
# managing local groups 
## managing group membership
```sh
groupadd sales 
usermod -G <group1>,<group2> <username> # change the user group 
usermod -aG <group> <username> # add user to group

gpasswd -M tux,test sales # add more than user to group

```
## making use of the SGID

## Group password


# using PAM 

# openLDAP 

# openLDAP Authentication 

# implementing Kerbros Authentication 
