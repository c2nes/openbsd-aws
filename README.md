
Build an OpenBSD AMI
====================

This project is an attempt to document a process for creating an OpenBSD AMI
from scratch from a released version of OpenBSD.

The rough process is,

1. Use a local OpenBSD install (physical or virtual) to build a `minirootXX.fs`
   ramdisk image with a bundled [autoinstall](http://man.openbsd.org/autoinstall)
   configuration.
2. Write this image to a EBS volume using an existing EC2 instance.
3. Attach the EBS volume as the root volume to a EC2 instance with another EBS
   volume attached to be the target of the installation.
4. Swap the EBS volumes and SSH to the new host.

Create ramdisk
--------------

1. Install OpenBSD.
2. Connect via SSH.
3. Configure doas,

        $ su -
        # echo allow nopass <username> > /etc/doas.conf
        # exit

4. Download source,

        $ ftp http://mirror.jmu.edu/pub/OpenBSD/6.0/src.tar.gz
        $ ftp http://mirror.jmu.edu/pub/OpenBSD/6.0/sys.tar.gz

5. Unpack source (as root),

        $ cd /usr/src
        $ doas tar -xzf ~/src.tar.gz
        $ doas tar -xzf ~/sys.tar.gz

6. Write out `auto_install.conf`,

        $ cd dist/amd64
        $ doas vi common/auto_install.conf

7. Include `auto_install.conf` in ramdisk,

        $ echo 'COPY    ${CURDIR}/../common/auto_install.conf   auto_install.conf' | doas tee -a common/list

8. Build,

        $ cd ../special
        $ doas make
        $ cd ../amd64
        $ doas make

9. Success,

        $ ls ramdisk_cd/obj/miniroot60.fs

