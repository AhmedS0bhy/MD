# Selinux basic configuration

```sh
getenforce      # get selinux status
setenforce 0    # set permissive mode
sestatus        # get persisitent and current settings
/etc/selinux/config # file for selinux config

getsebool -a                            # read booleans settings 
setsebool  <name of setting>            # set value

setmange boolen --list # read boolans setting from memory and file 
setsebool <setting name> <satus:on or off>  # set setting value
setsebool <setting name> <satus:on or off> -P  # set setting value on both memonry and file for perisistance 
```
## Avoiding relable all file system 
```sh
### After reset root password
load_policy -i # to load selinux ploicies 
restorecon -v /etc/shadow # restore the value of selinux for shadow files 
ls -Z /etc/shadow # make sure the changes applied  
```

# Controlling Users access 

```sh
id -Z # read SELinux context of user 
semanage login -l # list login
semange use -l      # list user 
semang login -C -l   # list changes
## -Z option used to asign a new user o a login with useadd 
```
# SELINUX type enforcement RULES
```sh
ps -Zp $(pgrep http)    # list the lable for process 
```
# SELinux log and audit 
```sh
ausearch -m AVC -c <command> -ts <time start> # saech for failure on logs 

## installing SElinux troubleshoot searver to get a log alerts 
yum install setroubleshoot-server 
sealert /var/log/message # get the solution 

## use the audit2why and audit2allow to get help 

semange permissive -a httpd_t # add domain exeptions 
```

# Create custom SELinux Modules 
```sh
yum install policycoreutils-devel # install the compilation module 
cp usr/share/selinux/devel/makefile <module directory > # copy the make file for selinux module to the custom module 
make private_files.pp # create a make file and compile the module 
semodule -i private_files.pp # import the SEmodule 
mkdir -m 1777 /no_guests # create a folder with stick bit 
chcon -t private_files_t /no_gursts # change the selinux stick on the folder 

semanage -r private_files # remove the modules
```