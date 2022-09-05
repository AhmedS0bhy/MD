# Install and initial information 
## install podman 
```sh
sudo yum install podman
```
## configured namespaces 
```sh 
sysctl -ar max_user_namespaces   # search for regEX for max_user
user.max_user_namespaces = 7093 # <- result

cat /etc/subuid /etc/subgid 
```
# Deploying Containers 

## basic podman command 
```sh
podman image ls # list podman images 
podman ps # running contaniner 
podman ps -a # list all the contaners 
podman rm <name> # remove the podman 
podman stop <name>

 
## inspect image ! get info 
podman inspect <image name>
# expose for port 


```
## searching for images 
```sh
podman search httpd # search for httpd image 

## list the configure registeries for search 
sed -E '/^(#|$)/d' /etc/containers/registries.conf

```
## deploying containers 
```sh
## run image on podman ##
podman run -it rhscl/httpd-24-rhel7 /bin/bash # -i for interactive -t to assign tty 

## port mapping
prodman run --name=www -d -p 8000:8080 <image name > #local:podman 

## enter existing running container
podman exec -it www /bin/bash
```

# managin container lifecycle 
## starting and stopping containers 
```sh
podman stop www     # stop container 
poman restart www   # restart contaniner 
podman kill www     # kill the cntainer 
podman restart -l # restart the last container 
podman stop -a # stop all 

## deleting container  
prodman rm www
prodman rm -f www # force delet 
prodman rm -a -f # all and force 

```
## connecting to running containers 

## deleting imanges 
```sh
podman rmi <image>
podman rmi -a  # remove all images 
```
## pulling containers
```sh
podman pull <name> # get the podman image 
```
## using port mapping 
```sh
podman run --name=www -d --publish-all <name image> # implement and publish all ports ! can get from "expose" 
                            ## ! open firewall port  ! ## 
firwall-cmd  --add-port=<port> 
```
## Creating presistent volumes 
```sh
chcon -Rt container_file_t <directory> # handle SELINUX profile 
prodman run --name=www -d --publish-all -v <directory on local host >:<podman dir> <image name>
```

# MAnging writable data 

## Envirounment values 
```sh 
prodman inspect <image> | grep usage 
```
## 1. identify UID and GID 
```sh
podman exec -it sql /bin/bash # drop into interacting shell
-> id  # get the service id
uid=27(mysql) gid=27(mysql) groups=27(mysql),0(root)
```
## 2. CReate volume for MYSQL data assignment 
```sh
mkdir /home/test/mysql # create the writable directory 
chcon -tR container_file_t /home/test/mysql # change the SELinux permission 
podman unshare chown <uid>:<uid> /home/test/mysql # share the uid with the host 
ls -ld /home/test/mysql # make sure of the work 
```
## 3. deploy SQL with perisistence for file system 
```sh
podman rm -f sql # remove the old container 
prodman run -d --name=sql -e MYSQL_USER=user \
-e MYSQL_PASSWORD=pass -e MYSQL_DATABASE=db \
-p 3306:3306 -v /home/test/mysql:/var/lib/mysql/data <image name> 
```

