# Installing CentOS7 
## Extra software for VBOX 
```sh 
yum install redhat-lsb-core net-tools epel-release kernel-headers kernel-devel 
yum groupinstall "Development Tools"
yum update 
reboot 
```
## Install GUI 
```sh
yum groupinstall "X Window System" "MATE Desktop" # install mate desktop 
systemctl set-default graphical.target # set the graphical to be the default target 
systemctl isolate graphical.target # move the graphic without reboot 
```
# Working at the Command Line 
## listing files 
```sh
ls -l   # long list
ls -i   # get the inode 
ls -lh  # get human readable format 
ls -a   # list all include hidden files 
ls -ltr # long list and sort with timestamp 
ls -F   # file type 
```
## file types 
```sh
-   # files 
D   # directory 
L   # for link 
B   # for block devices 
C   # character devices 
N   # named pipes 
S   # Sockets  
```
## Working with files 
```sh
cp      # copy file  
mv      # cut files 
rm      # remove 
#------using reg-----------#
*       # for any number and anything after 
cp *.md # copy any thing that have .md
?       # represent a single char or number 
[]      # group of char or chracters 

```
## working with directories 
```sh
mkdir           # create a folder 
mkdir -p        # create the parent folders
mkdir -m        # set permmistion 

rmdir           # remove empty folder 
rm -rf          # remove folder with the contnet on it 
cp -R sales     # copy folder with content 

```
## working with links 
```sh 
ln          # Create links 
ln -s       # smblik link or soft link 
```
# Reading files 
## list file 
```sh
cat 
less # using / to search forword ? for backword 
head # display top 10 line 
tail # display the last 10 line 
tail -n1 # identify the number of lines 
```
## regular expressions and grep 
```sh
grep # used to search for text 
grep -E # for using regular expressiong 
#--------------------#
^   # begin with 
$   # end with 
^$  # empty lines 
#-----examples-------#
grep - E '[aeiou]{5}' /filename # search for anything inside [] match it {} time 
```
## using sed 
```sh
sed -i # edit files 
sed -i '/#/d;/^$/d' $1 # /d for delete 
```
## comparing files 
```sh
diff # get the diffrent between two files 
diff ntp.conf /etc/ntp.conf 

md5sum # compare binary files 

```
## finding files 
```sh
find /etc -maxdepth 1 -type l # saerch /etc directory for links 
find /boot -type f -size +1000k # sarch for files using size 
fid /usr/share -name 'filename' -exec cp {} .\ # saerch for file name and after get it execute cp {filename} 
-i # for case insenstive  
```

# using Vim 
## Vimtutor 
```sh
vimtutor # get the document to use vim 
```
## setting defaults in .vimrc 
```sh
set showmod # show the modeis currentlyy in 
set nonumber # no number 
set hlsearch # hiighligh search 
set ai # auto indent 
set ts=4 expandtab #create tab spaces 
abbr _sh \#!/bin/bash<CR># create a abbration 
nmap <C-N> :set invnumber<CR> # set numbering on and off `ctrl and N`
```
```sh
shotrcuts get it from eng abeer notes plz !!!
```

# piping and Redirection 
## Redirecting STDOU
```sh
ls > file1  # redirect ouput 
ls 1> file  #  the same 
```
## Redirecting STDERR
```sh
find /etc -type 1 2> /dev/null 
find /etc -type 1 &> file # redirect the error and output both 
```
## Reading into STDIN
```sh
mail -s "disk free" tux < diskfree
```
## using here documents 
```sh
cat > newfile <<END
> this is new file 
and is created on the 
coman line 
END 
```

## command pipelines 
```sh
cut -f7 -d : /etc/passwd | sort | uniq 
```
## named pipes 
```sh
mkfifo mypip # create a fifo file 
ls > mypip # direct the ouput to fifo file 
wc -l < mypip # get data from fifo file
```
## using tee 
```
ls | tee file1 # redirect the output and print it to stdout 
```
# Archiving files 
## using tar 
```sh
tar -fc # create F for file 
tar -ft # testing 
tar -fx # expand 

```
## Compression 
```sh
## gzip
gzip      # compress 
gunzip    # unzip 
tar -z test.tar.gz # using tar and gzip 
```
## cpio 
```sh
cp /boot/initramfs.img /tmp 
mkdir /tmp/work 
cd /tmp/work
cpio -id < initramfs.img 
```
## imaging with dd 
```sh
dd if=/dev/sr0 of=/tmp/disk.iso # imaging a CD 
```

# Accessing Help 
## MAN pages
```sh
man pages 
1 -> usage 
5 -> configuration and file formate 
8 -> administrative command 
```
# File permissions
## listing permissions 
```sh
ls -l 
stat -c %a 
stat -c %A
```
## managing default permissions 
```sh

``` 
## setting permission 
```sh
chmod 740 file 
1 -> execute 
3 -> read
4 -> write  
```
## managing file ownership 
```sh
chown user:group file 
```
# Accessing Servers with SSH 

## using key based authentication 
```sh
ssh-keygen -t rsa # create key with type rsa 
ssh-copy-id -i id_rsa.pub <server># copying the public key to server 
ssh-agent bash  # authonicate to save the passphrase 
ssh-add # adding private key 

 ```
## remove root login 
```sh 
vim /etc/ssh/ssh_d_config
/PermitRootLogin 
PermitRootLogin without-password
``` 
## copy files securely 
```sh
scp fileToCopy 192.168.1.2:/tmp # copy from client to server 
scp 192.168.1.2:/fileToDown /home/sobhy
```
# using screen and script
