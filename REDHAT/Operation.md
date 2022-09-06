# Starting and stopping CentOS
## changing Runlevels 
```sh
who -r 
runlevel 
systectl get-default 
systemctl set-default 
```
## Selecting runlevels at boot 
1. ESC in GRUB 
2. search for linux16 line and `ctrl + e` to go to the end of line 
3. append systemd.unit=rescure.target 
4. `ctrl + x` to continure to boot 

# The Boot process
## managing GRUB Recovery
1. vi /etc/default/grup 
2. search and set  `GRUB_DISABLE_RECOVERY="false"`
3. run `grub2-mkconfig -o /boot/grub/grub.cfg` 
## Reset Lost Root passwords 
1. ESC in GRUB 
2. search for linux16 line and `ctrl + e` to go to the end of line 
3. Remove `rhgb` and `quiet` 
4. add `rd.break enforcing=0`
5. mount the root `mount -o remount,rw /sysroot; chroot /sysroot`
6. run `passwd; exit`
7. remount the sysroot `mount -o remount,ro /sysroot`
8. exit to continure 
9. selinux lable `restorecon /etc/shadow` 
10. reset the SELINUX `setenforce 1  `

# Managaing Grub2
## reinstall and repair GRUB2
```sh
grub2-install /dev/sda # <bootable device>

yum reinstall grub2-efi shim # for efi 
```
## Manage Grub2 Defaults
```sh
/etc/default/grub # file for grub configuration 
grub2-mkconfig -o /boot/grub/grub.cfg # change the configuration on boot 
```
## using grubby 
```sh 
grubby --default-kernel 
grubby --set-default
grubby --info=ALL
grubby --info /boot/vmlinuz 
grubby --remove-args "rhrd" 
```
## password protect GRUB2
```sh
vi /etc/grub.d/01_users 
```
```sh
#!/bin/sh -e 
cat << EOF
    set superusers="user"
    password andrew L1nux
EOF
```
```sh
grub2-mkconfig -o /boot/grub/grub.cfg # make the password configuration change 
```

# Linux Processes
## listing processes 
```sh
ps -elf # all process long and full 

```
## pgrep, pkill 
### kill 
```sh
kill -l         # list signals 
kill -15 PID    # terminate
kill -9 PID     # force kill 
ctrl + c        # intrupt 
ctrl + z        # suspend 

pgrep <proc name> # get process ID
pkill <proc name> # kill process with name 
ps -F -p $(gpgrep <proc name>) # get full list for process using name 
```

# process priority 
## backgrounding task 
```sh
& # working at background 
^z bg # suspind the process ;bg to work at background 
jobs # list background task 
fg # bring it to forground
```
## using nice 
```sh 
nice value # start from -20 to +19 
process priority 60 - 99 
nice -n 5 firefox& # start with nice value 
renice -n 10 -p pid # renice running proc 
/etc/security/limits.conf # defualt values for users and group 
-> tux - priority 10 <- 

```

# monitor linux performance 
## using pwdx and pmap 
```sh
#----------memory-------#
free # list the free memory 
free -m # list free with megabyte 
pmap <pid> # get the memory addresses for lib 

pwdx <pid> # get file system directory for process 
```
## working with uptime and tload 
```sh 
uptime # get the time for system uptime 
lscpu # get the cpu information 
```
## recording performance with top and vmstat
```sh
top # display information 
top -b -n1 # just display one time
 
vmstat # print information about system 
vmwstat -S m # print in megabyte format  
```

# Using sysstat to monitor 
## install the package 
```sh
yum install sysstat  #install systat package 

cat /etc/cron.d/sysstat # see the jobs of collection 

cat /etc/sysconfig/sysstat # review the sysstat configuration 

systemctl enable --now sysstat # start and enable the service 
```  
## additional tools 
```sh
iostat              # disk activity 
iostat -m           # data in megabyte 
iostat -m 5 3       # run it 3 time in gap of 5 sec

pidstat -p <pid> 5 3   # collecting the usage of process 

mpstat -P ALL 2 3   # get information about the CPU 

```
## creating system Report 
```sh
sar -u  # Cpu information 
sar -r  # reporting on memory 
sar -p  # disk IO 
sar -n  # network conectivity 
/var/log/sa # file for log for every day  
sar -r <time> -e <ToTime> -f </var/log/sa/sa15 diffrent file>

```

# Scheduling tasks
## using cornd 
```sh
systemctl status crond # check the serivce 

# files and directory 
/etc/crontab
/etc/cron.d 

# using cron 
crontab -l # list the jobs 
vrontab -e # adding a task 
crontab -r # removing task 

# timeing fields
min hour d-o-m month dow
-> * mean all  
```
## using anacron 
```sh 
/etc/anacrontab 
# format 
period delay job-id-name command 
```
## using at 
```sh
systemctl status atd 
at clock month year 
atq # list jobs
atrm # remove jobs 
```

# log files and logrotate 
## auditing user logins 
```sh
lastlog | grep -v "Never" # print the users last login 

last -n 10 # get last 10 lines information 
last | grep "still" # grep the still login users 
last reboot # get the reboot events 
last tux    # get user login activity 
lastb       # last bad login faiuld  password 
```
## auditing root access 
```sh
/var/log/secure # directoy to log files # have the su and sudo event 

cat secure | grep sudo # get all sudo action 
cat secure | grep su    # get all changing users session 
```
## using awk 
```sh 
    search    operation 
       |     action|feild      
awk '/sudo/ { print $0 } ' secure 
awk '/sudo/ { print $5,$6 }' secure 
```
## Configuring rsyslogd 
```sh
/etc/rsyslog.conf # sysconfiguration 
```
## managing logs with journalctl 
```sh
journalctl 
journalctl -n # last entrys like less 
journalctl -f # follow the log 
journalctl -b # for boot 

journalctl -u sshd.service # for special unit 

journalctl --since "2022-01-31 12:00:00" 
journalctl --since "10 minutes ago"

```
```sh
#----- make journald perisenstnce ----# 
mkdir /var/log/journal 
vim /etc/systemd/journald.conf
-> Storage=persistent 
reboot

journalctl --list-boots 
journalctl -b -1 

```

# introducting SELinux 
## SELinux status
```sh
sestatus    # get full info  
getenforce  # get current setting 
setenforce 0 | 1 # 0 permissive ,1 enfoce 
/etc/selinux/config # the conf file 

# get SELinux type 
ls -Z <file>
ps -Z <pid>
id -Z 
```
## audit logs 
```sh
/var/log/audit/audit.log # log file
ausearch -m avc # search for avc 
ausearch -m avc -ts recent # define the time stamp to search 
restorecon /etc/shadow # resote the lable for the file 
chcon -t shadow_t /etc/shadow # manual change the con 
```
## booleans and ports 
```sh 
getsebool -a # get all bools and conf 
semanage boolean -l 

setsebool httpd_read_user_content on # change boolean 
setsebool httpd_read_user_content on  -P # for permaneny 
semanage port -l # list all define ports 
semange port -at ssh_port_t -p tcp 1111 # change and add port 
```

# managing software 
## using rpm 
```sh
rpm -qa  # list all rpm
rpm -i <name.rpm>
rpm -e nmap # remove package
rpm ql nmap # quere package 
rpm -qpl <name.rpm> # list pacakge
rpm -V nmap # verify the rpm 
```
## working with yum
```sh
yum install bash-completion # install package 
yum info bash-completion # get information about the package
yum list installed # list the packages 
yum remove bash-complations # remve the package and dependency 
``` 
## configure software repo 
```sh
/etc/yum.repo.d/

yum repolist        # list enabled repo
yum repolist all    # list all repo 
```

