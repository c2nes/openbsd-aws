
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
        $ doas make
9. Success,
        $ ls ramdisk_cd/obj/miniroot.fs

