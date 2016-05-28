#Creación de Ambiente Openstack.
## Preparando ambiente.
Para este laboratorio usaremos vm's basadas en RHEL 7.2 sobre Fedora 23  KVM Nested.

### 1.- Lo primero que realizaremos es descargar la imagen oficial desde el portal de Red Hat a nuestro equipo.

        [root@localhost images]# curl -o /var/lib/libvirt/images/rhel7-guest-official.qcow2 https://access.redhat.com/downloads/content/69/ver=/rhel---7/7.1/x86_64/product-downloads

### 2 .- Checamos la información de la imagen que acabamos de descargar.

        [root@localhost images]# qemu-img info rhel-guest-image-7.2-20160302.0.x86_64.qcow2 
        image: rhel-guest-image-7.2-20160302.0.x86_64.qcow2
        file format: qcow2
        virtual size: 10G (10737418240 bytes)
        disk size: 470M
        cluster_size: 65536
        Format specific information:
                compat: 0.10
                refcount bits: 16

Sí investigamos un poco más nos podemos dar cuenta que la imagen solo tiene 6GB de almacenamiento disponible.

        [root@localhost images]# virt-filesystems --long -h --all -a rhel-guest-image-7.2-20160302.0.x86_64.qcow2
        Name       Type        VFS  Label  MBR  Size  Parent
        /dev/sda1  filesystem  xfs  -      -    6.0G  -
        /dev/sda1  partition   -    -      83   6.0G  /dev/sda
        /dev/sda   device      -    -      -    10G   -
        
### 3.- Vamos a tener que cambiar el tamaño de la imagen para tener un poco más de espacio para los laboratorios posteriores.

La herramienta virt-resize permite cambiar el tamaño de la partición y el sistema de archivos al mismo tiempo, pero no funciona sobre la imagen original, por lo que es necesario crear una nueva imagen base:

        [root@localhost images]# qemu-img create -f qcow2 OSP82.qcow2 100G
        Formatting 'OSP82.qcow2', fmt=qcow2 size=107374182400 encryption=off cluster_size=65536 lazy_refcounts=off refcount_bits=16

### 4 .- Con el fin de cambiar automáticamente el tamaño del sistema de ficheros XFS, virt-resize requiere libguestfs-xfs:

        [root@localhost images]# dnf -y install libguestfs-xfs

### 5.- Ahora ejecutamos virt-resize:

        [root@localhost images]# virt-resize --expand /dev/sda1 rhel-guest-image-7.2-20160302.0.x86_64.qcow2 OSP81.qcow2 
        [   0.0] Examining rhel-guest-image-7.2-20160302.0.x86_64.qcow2
        ◓ 25% ⟦▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒════════════════════════════════════════════════════════════════⟧ --:--
         100% ⟦▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒⟧ --:--
        **********
        
        Summary of changes:
        
        /dev/sda1: This partition will be resized from 6.0G to 100.0G.  The 
        filesystem xfs on /dev/sda1 will be expanded using the 'xfs_growfs' method.
        
        **********
        [  21.8] Setting up initial partition table on OSP81.qcow2
        [  21.9] Copying /dev/sda1
         100% ⟦▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒⟧ 00:00
        [  38.8] Expanding /dev/sda1 using the 'xfs_growfs' method
        
        Resize operation completed with no errors.  Before deleting the old disk, 
        carefully check that the resized disk boots and works correctly.


Verificamos los cambios realizados a nuestro Disco y deberiamos observar que el tamaño es de 100G:

        [root@localhost images]# qemu-img info  OSP81.qcow2 
        image: OSP81.qcow2
        file format: qcow2
        virtual size: 100G (107374182400 bytes)
        disk size: 1.1G
        cluster_size: 65536
        Format specific information:
            compat: 1.1
            lazy refcounts: false
            refcount bits: 16
            corrupt: false


Validamos los cambios el el filesystem:

        [root@localhost images]# virt-filesystems --long -h --all -a OSP81.qcow2 
        Name       Type        VFS  Label  MBR  Size  Parent
        /dev/sda1  filesystem  xfs  -      -    100G  -
        /dev/sda1  partition   -    -      83   100G  /dev/sda
        /dev/sda   device      -    -      -    100G  -

### 6 .- A continuación, vamos a eliminar la cloud-init desde guest-image; no vamos a necesitar este servicio,

ya que no estamos usando esta máquina dentro de un entorno que admite un servicio de metadatos, además de que también se ralentiza considerablemente el proceso de arranque:

        [root@localhost images]# virt-customize -a OSP81.qcow2  --run-command 'yum remove cloud-init* -y'
        [   0.0] Examining the guest ...
        [  13.9] Setting a random seed
        [  13.9] Running: yum remove cloud-init* -y
        [  17.9] Finishing off


### 7.- Ahora procederemos a cambiar el password default del usuario root:

        [root@localhost images]# virt-customize -a OSP81.qcow2 --root-password password:redhat2016
        [   0.0] Examining the guest ...
        [  12.7] Setting a random seed
        [  12.7] Setting passwords
        [  14.0] Finishing off

### 8.- Tenemos que asegurarnos de que nuestra máquina Idm tiene una interfaz 2 interfaces de red por defecto,

por lo que crearemos un archivo de configuración para la interfaz para eth1 basado en la eth0:

        [root@localhost images]# virt-customize -a OSP82.qcow2 --run-command 'cp /etc/sysconfig/network-scripts/ifcfg-eth{0,1} && sed -i s/DEVICE=.*/DEVICE=eth1/g /etc/sysconfig/network-scripts/ifcfg-eth1'
        [   0.0] Examining the guest ...
        [  11.9] Setting a random seed
        [  11.9] Running: cp /etc/sysconfig/network-scripts/ifcfg-eth{0,1} && sed -i s/DEVICE=.*/DEVICE=eth1/g /etc/sysconfig/network-scripts/ifcfg-eth1
        [  12.1] Finishing off

### 9.- Procederemos a crear nuestra vm en KVM con la imagen OSP81.qcow2 que acabamos de crear.

* Creación de vm.

![GitHub Logo](/img/Lab-1/Lab-1-a.png)

* Seleccionamos el Disco que creamos.

![GitHub Logo](/img/Lab-1/Lab-1-i.png)

* Asignamos tipo y Versión de S.O.

![GitHub Logo](/img/Lab-1/Lab-1-c.png)

* Asignamos memoria y CPU

![GitHub Logo](/img/Lab-1/Lab-1-d.png)

* Personalizamos la configuración (Agregaremos la segunda interfaz de red)

![GitHub Logo](/img/Lab-1/Lab-1-e.png)

* Asignamos la segunda interfaz de red que configuramos.

![GitHub Logo](/img/Lab-1/Lab-1-f.png)

* Terminamos el proceso de creación, iniciamos la vm, realizamos loggin y validamos interfaces de red.

![GitHub Logo](/img/Lab-1/Lab-1-g.png)
