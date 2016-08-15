## Configurando Red

        [chakanais@localhost ~]$ ssh root@192.168.122.12
        Last login: Sat May 28 13:20:44 2016
        [root@localhost ~]# systemctl disable NetworkManager
        Removed symlink /etc/systemd/system/multi-user.target.wants/NetworkManager.service.
        Removed symlink /etc/systemd/system/dbus-org.freedesktop.NetworkManager.service.
        Removed symlink /etc/systemd/system/dbus-org.freedesktop.nm-dispatcher.service.
        [root@localhost ~]# systemctl stop NetworkManager
        [root@localhost ~]# chkconfig network on
        [root@localhost ~]# service network start
        Starting network (via systemctl):                          [  OK  ]

## Configurando nombre del equipo

        [root@localhost ~]# hostnamectl set-hostname osp81.poc.redhat.com
        [root@localhost ~]# hostnamectl set-hostname "Mike's lab for demos osp81 packstack" --pretty 
        [root@localhost ~]# hostnamectl status
           Static hostname: osp81.poc.redhat.com
           Pretty hostname: Mike's lab for demos osp81 packstack
                 Icon name: computer-vm
                   Chassis: vm
                Machine ID: 02f1ddb1415c4feba9880b2b8c4c5925
                   Boot ID: 0d4042cdf86e442bbb1b55823169da0a
            Virtualization: kvm
          Operating System: Red Hat Enterprise Linux Server 7.2 (Maipo)
               CPE OS Name: cpe:/o:redhat:enterprise_linux:7.2:GA:server
                    Kernel: Linux 3.10.0-327.10.1.el7.x86_64
              Architecture: x86-64

## Registrando el equipo

        [root@localhost ~]# subscription-manager register --username user --password password! --auto-attach
                Registering to: subscription.rhn.redhat.com:443/subscription
                The system has been registered with ID: 27d763fb-a2d3-4e8a-8e0f-6602d785d9a0 
                
                Installed Product Current Status:
                Product Name: Red Hat Enterprise Linux Server
                Status:       Subscribed

        [root@localhost ~]# subscription-manager list --available | grep -i OpenStack
                SKU:                 SER0412
                Contract:            10748248
                Pool ID:             8a85f9814ed5ff68014ed6415bc0722c
                Provides Management: No
                Available:           4
                Suggested:           1
        
                     Red Hat OpenStack
                     Red Hat OpenStack Beta
                     Red Hat OpenStack Beta Certification Test Suite

        [root@osp81 ~]# subscription-manager list --available | grep -A8 "Red Hat Enterprise Linux Server"
                SKU:                 SER0412
                Contract:            10748248
                Pool ID:             8a85f9814ed5ff68014ed6415bc0722c
                Provides Management: No
                Available:           4
                Suggested:           1
                                     Red Hat Enterprise Linux Server

        [root@osp81 ~]# subscription-manager attach --pool=8a85f9814ed5ff68014ed64355e92fb0

        [root@osp81 ~]# subscription-manager repos --list | grep -i rhel-7-server-openstack-8
                Repo ID:   rhel-7-server-openstack-8-optools-source-rpms
                Repo ID:   rhel-7-server-openstack-8-source-rpms
                Repo ID:   rhel-7-server-openstack-8-director-rpms
                Repo ID:   rhel-7-server-openstack-8-tools-source-rpms
                Repo ID:   rhel-7-server-openstack-8-tools-rpms
                Repo ID:   rhel-7-server-openstack-8-tools-debug-rpms
                Repo ID:   rhel-7-server-openstack-8-director-debug-rpms
                Repo ID:   rhel-7-server-openstack-8-director-source-rpms
                Repo ID:   rhel-7-server-openstack-8-rpms
                Repo ID:   rhel-7-server-openstack-8-debug-rpms
                Repo ID:   rhel-7-server-openstack-8-optools-debug-rpms
                Repo ID:   rhel-7-server-openstack-8-optools-rpms




        [root@localhost ~]# subscription-manager repos --enable=rhel-7-server-rpms \
        > --enable=rhel-7-server-openstack-8-rpms \
        > --enable=rhel-7-server-optional-rpms \
        > --enable=rhel-7-server-rh-common-rpms \
        > --enable=rhel-7-server-openstack-8-optools-rpms
                Repository 'rhel-7-server-rpms' is enabled for this system.
                Repository 'rhel-7-server-optional-rpms' is enabled for this system.
                Repository 'rhel-7-server-openstack-8-optools-rpms' is enabled for this system.
                Repository 'rhel-7-server-openstack-8-rpms' is enabled for this system.
                Repository 'rhel-7-server-rh-common-rpms' is enabled for this system.

        [root@osp81 ~]# yum install openstack-packstack.noarch
        Dependencias resueltas
        
        ================================================================================================
         Package                                             Arquitectura                    Versión                                                         Repositorio                                             Tamaño
        ================================================================================================
        Instalando:
         openstack-packstack
         (...)
         (...)

## Deploy Packstack AIO.

        [root@osp81 ~]# ssh-keygen -t rsa
        Generating public/private rsa key pair.
        Enter file in which to save the key (/root/.ssh/id_rsa): 
        Enter passphrase (empty for no passphrase): 
        Enter same passphrase again: 
        Your identification has been saved in /root/.ssh/id_rsa.
        Your public key has been saved in /root/.ssh/id_rsa.pub.
        The key fingerprint is:
        6c:aa:b1:b5:fa:a1:99:9f:f7:0d:a3:ef:44:96:54:3a root@osp81.poc.redhat.com
        The key's randomart image is:
        +--[ RSA 2048]----+
        |            .    |
        |           o     |
        |          E      |
        |       . . o     |
        |        S +      |
        |       o o       |
        |    . +   +      |
        |     O +.o +     |
        |    B+=..++ .    |
        +-----------------+


### En el nodo 2 instalar por separado los siguientes paquetes

        [root@osp81 ~]# openstack-ironic-common and python-ironicclient
        [root@osp81 ~]# packstack --gen-answer-file=ans.txt
        [root@osp81 ~]# packstack --answer-file=ans.txt

