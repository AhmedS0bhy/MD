# Install XRDP 
```sh
yum install xrdp 

cd /usr/sbin 
chcon -t bin_t xrdp xrdp-sesman # change the SELINUX 

vim /etc/xrdp/startwm.sh  # if we work with default desktop no need that 
echo 'mate-session' > ~/.xsession 
chmod +x ~/.xsession 
```
# Virtual Machine Networking 
```sh
virtsh net-define 
virsh net-sart 
virsh net-autostart 
virsh net-edit 

brctl show
brctl addbr 
```
# Install KVM
```sh
yum install qemu-kvm virt-install virt manager 

qemu:///system 
qemu_ssh://user@host/system 
```
# Creating a virtual Machine 
```sh
virt-install 
virt-viewer 
virsh net-edit 

virt-install --hvm --connect <localhost> --network=bridge:virbr0 --pex --graphics spice --name=centos/-vm --ram= 512 --vcpu=1 os-type=linux --os-variant=rhel7 --disk path=/var/lib/libvirt/images/centos7-vm.qcow2,size=9
```
# Configure VM 
```sh
sudo virt-install --hvm --conect qemu:///system --network=bridge:virbr0,mac=52:54:00:00:00:01 --pxe --graphics spice --name=bob --ram 512 --vcpus=1--os-type=linux --os-variant=rhel7 --disk path=/var/lib/libvirt/images/bob/bob.qcow2,size=5 
```
## Using net-update 
```sh
virsh net-update default add ip-dhcp-host "<host mac='mac' name='bob' ip='192.168.1.11'/>"  --live --config 
```
## VM control 
```sh 
virsh dominfo <name>
virsh shutdow <name>
virsh reboot alice 
virsh destroy alice 
virsh undefine alice --remove-all-storage 
```
## Montring 
```sh
virt-top 
```
## adding resource 
```sh 
virtsh setmaxmem alice 1500m --config 
virsh setmem alice 728 --live 
```
# Docker 
```sh
yum install docker 

docker info 
docker version 

docker run hello-world 

docker start -a 

docker run -it ubuntu bash 

docker run --name d1 -p 80:80 -v /html:var/www:ro -d nginx 
```