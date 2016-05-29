    1  ifconfig 
    2  cd /etc/sysconfig/network-scripts/
    3  cat ifcfg-eth1 
    4  history 
    5  systemctl disable NetworkManager
    6  ystemctl stop NetworkManager
    7  systemctl stop NetworkManager
    8  chkconfig network on
    9  service network start
   10  hostnamectl set-hostname osp81.poc.redhat.com
   11  ifconfig eth0
   12  hostnamectl set-hostname "Mike's lab for demos osp81 packstack" --pretty 
   13  hostnamectl status
   14  subscription-manager register --username m.pineda --password redhat2014! --auto-attach
   15  =================================================================================
   16  ================================================================================
   17  =================================================================================
   18  =================================================================================4
   19  =================================================================================
   20  v
   21  subscription-manager list --available
   22  subscription-manager list --available | grep -i open
   23  subscription-manager list --available | grep -i OpenStack
   24   subscription-manager list --available | grep -A8 "Red Hat Enterprise Linux Server"
   25  subscription-manager list --available | grep -A8 "Red Hat Enterprise Linux OpenStack Platform"
   26  subscription-manager list --available | grep -i OpenStack
   27  subscription-manager attach --pool=8a85f9814ed5ff68014ed64355e92fb0
   28  history 
   29  subscription-manager repos --enable=rhel-7-server-rpms --enable=rhel-7-server-openstack-8-rpms --enable=rhel-7-server-optional-rpms --enable=rhel-7-server-rh-common-rpms --enable=rhel-7-server-openstack-8-optools-rpms
   30  yum install yum-plugin-priorities yum-utils 
   31  reboot 
   32  ifconfig 
   33  cd /etc/sysconfig/network-scripts/
   34  vi ifcfg-eth0
   35  service network restart
   36  ifconfig 
   37  ethtool eth0
   38  ethtool eth1
   39  journalctl -xe
   40  route /n
   41  route -n
   42  cat /etc/resolv.conf 
   43  init 0
   44  ifconfig 
   45  ifdown eth0
   46  ifconfig 
   47  init 0
   48  ifconfig 
   49  yum update
   50  ssh-copy-id 
   51  ssh-keygen -t rsa
   52  packstack --help | less
   53  packstack --gen-answer-file=/root/osp81.txt
   54  vi /root/osp81.txt 
   55  packstack --help | less
   56  packstack --answer-file=/root/osp81.txt
   57  vi /root/osp81.txt 
   58  packstack --answer-file=/root/osp81.txt
   59  vi /root/osp81.txt 
   60  packstack --answer-file=/root/osp81.txt
   61  ssh-copy-id root@192.168.122.12
   62  ssh root@192.168.122.12
   63  vi /etc/hosts
   64  packstack --answer-file=/root/osp81.txt
   65  vi /root/osp81.txt 
   66  ssh-copy-id root@192.168.122.11
   67  ssh root@192.168.122.11
   68  packstack --answer-file=/root/osp81.txt
   69  vi /root/.ssh/known_hosts 
   70  packstack --answer-file=/root/osp81.txt
   71  ssh-keygen -t rsa
   72  packstack --answer-file=/root/osp81.txt
   73  subscription-manager remove --all
   74  subscription-manager unregister
   75  subscription-manager clean
   76  subscription-manager register --username m.pineda --password redhat2014!
   77  subscription-manager subscribe --pool=8a85f9814ed5ff68014ed64355e92fb0
   78  subscription-manager list --available | grep -i OpenStack
   79  subscription-manager repos --disable=*
   80  cat /var/log/rhsm/rhsm.log
   81  ifconfig 
   82  ping 8.8.8.8
   83  service network restart
   84  ifconfig 
   85  ping 8.8.8.8
   86  vi /etc/sysconfig/network-scripts/ifcfg-eth1 
   87  service network restart
   88  systemctl restart network
   89  vi /etc/sysconfig/network-scripts/ifcfg-eth1 
   90  service network restart
   91  vi /etc/sysconfig/network-scripts/ifcfg-eth0
   92  service network restart
   93  systemctl restart network
   94  ping 8.8.8.8
   95  subscription-manager repos --disable=*
   96  subscription-manager repos --enable=rhel-7-server-rpms  --enable=rhel-7-server-openstack-8-rpms --enable=rhel-7-server-optional-rpms  --enable=rhel-7-server-rh-common-rpms
   97  subscription-manager repos --list | grep -i rhel-7-server-openstack-8
   98  subscription-manager repos --enable=rhel-7-server-openstack-8-optools-rpms --enable=rhel-7-server-openstack-8-tools-rpms --enable=rhel-7-server-openstack-8-director-rpms
   99  ssh 192.168.122.12
  100  ssh-copy-id root@192.168.122.11
  101  ssh-copy-id root@192.168.122.12
  102  ssh 192.168.122.12
  103  packstack --answer-file=/root/osp81.txt
  104  systemctl status httpd.service
  105  ps aux | grep httpd
  106  systemctl restart httpd
  107  vi /etc/httpd/conf.d
  108  openstack service list
  109  setenforce 0
  110  getenforce
  111  /bin/systemctl start  httpd.service
  112  ausearch -m avc -m user_avc -m selinux_err -i -ts today
  113  date
  114  yum -y install policycoreutils-python
  115  semanage port -a -t http_port_t -p tcp 5000
  116  semanage port -l | grep 5000 |grep tcp
  117  semanage port -a -t http_port_t -p tcp 5030
  118  vi /etc/httpd/conf/ports.conf
  119  vi /etc/httpd/conf.d/10-keystone_wsgi_main.conf 
  120   service httpd start
  121  systemctl status httpd.service
  122  cat /var/tmp/packstack/20160528-203612-2H38eQ/manifests/192.168.122.11_horizon.pp.log | less
  123  cd /etc/pki/tls/certs
  124  ls -ltrh
  125  cat ssl_dashboard.crt 
  126  cat /root/osp81.txt 
  127  history 
