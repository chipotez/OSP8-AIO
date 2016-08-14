
cd /etc/sysconfig/network-scripts/
cat ifcfg-eth1 
systemctl disable NetworkManager
systemctl stop NetworkManager

chkconfig network on
service network start

hostnamectl set-hostname osp81.poc.redhat.com

hostnamectl set-hostname "Mike's lab for demos osp81 packstack" --pretty 
hostnamectl status

subscription-manager register --username user --password password --auto-attach
subscription-manager list --available


subscription-manager list --available | grep -A8 "Red Hat Enterprise Linux Server"
subscription-manager list --available | grep -A8 "Red Hat Enterprise Linux OpenStack Platform"
subscription-manager list --available | grep -i OpenStack

subscription-manager attach --pool=8a85f9814ed5ff68014ed64355e92fb0
subscription-manager repos --enable=rhel-7-server-rpms --enable=rhel-7-server-openstack-8-rpms --enable=rhel-7-server-optional-rpms --enable=rhel-7-server-rh-common-rpms --enable=rhel-7-server-openstack-8-optools-rpms
yum install yum-plugin-priorities yum-utils 
yum update
reboot 


cd /etc/sysconfig/network-scripts/
vi ifcfg-eth0
service network restart
 
ethtool eth0
ethtool eth1

journalctl -xe
route -n
cat /etc/resolv.conf 
vi /etc/hosts

ssh-keygen -t rsa
packstack --gen-answer-file=/root/osp81.txt
vi /root/osp81.txt 
packstack --answer-file=/root/osp81.txt

ssh-copy-id root@192.168.122.12
ssh root@192.168.122.12


subscription-manager remove --all
subscription-manager unregister
subscription-manager clean


subscription-manager register --username m.pineda --password redhat2014!
subscription-manager subscribe --pool=8a85f9814ed5ff68014ed64355e92fb0
subscription-manager list --available | grep -i OpenStack
subscription-manager repos --disable=*
subscription-manager repos --enable=rhel-7-server-rpms  --enable=rhel-7-server-openstack-8-rpms --enable=rhel-7-server-optional-rpms  --enable=rhel-7-server-rh-common-rpms
subscription-manager repos --list | grep -i rhel-7-server-openstack-8
subscription-manager repos --enable=rhel-7-server-openstack-8-optools-rpms --enable=rhel-7-server-openstack-8-tools-rpms --enable=rhel-7-server-openstack-8-director-rpms

openstack service list
setenforce 0
getenforce
/bin/systemctl start  httpd.service
ausearch -m avc -m user_avc -m selinux_err -i -ts today
date

yum -y install policycoreutils-python
semanage port -a -t http_port_t -p tcp 5000
semanage port -l | grep 5000 |grep tcp
semanage port -a -t http_port_t -p tcp 5030

vi /etc/httpd/conf/ports.conf
vi /etc/httpd/conf.d/10-keystone_wsgi_main.conf 
 service httpd start
systemctl status httpd.service
cat /var/tmp/packstack/20160528-203612-2H38eQ/manifests/192.168.122.11_horizon.pp.log | less
cd /etc/pki/tls/certs
ls -ltrh
cat ssl_dashboard.crt 
cat /root/osp81.txt 
