# Debian启动自动挂载windows分区.md

1. 显示windows分区的uuid
    `ls -l /dev/disk/by-uuid`

    例：
    ```
    niuniu@debian:~$ ls -l /dev/disk/by-uuid
    lrwxrwxrwx 1 root root 10 Jul 21 23:08 78B84D2CB84CE9E8 -> ../../sda8
    ```
2. 使用sudo编辑/etc/fstab文件，将第1步获取到的uuid按照fstab中既有的格式添加到配置中
    例：
    ```bash
    # /etc/fstab: static file system information.
    #
    # Use 'blkid' to print the universally unique identifier for a
    # device; this may be used with UUID= as a more robust way to name devices
    # that works even if disks are added and removed. See fstab(5).
    #
    # <file system> <mount point>   <type>  <options>       <dump>  <pass>
    # / was on /dev/sda4 during installation
    UUID=a55a1627-e6ed-40f7-bfa4-5bd8ed3e8ca6 /               ext4    errors=remount-ro 0       1
    # /boot/efi was on /dev/sda1 during installation
    UUID=9489-DCC5  /boot/efi       vfat    umask=0077      0       1
    # /home was on /dev/sda5 during installation
    UUID=b6bc1c08-d823-48e3-a1b9-2e852441f606 /home           ext4    defaults        0       2
    # /opt was on /dev/sda2 during installation
    UUID=54d9abd9-7fce-4df4-a3e5-7a441eba453e /opt            ext4    defaults        0       2
    # swap was on /dev/sda3 during installation
    UUID=7eda503d-edcc-4bee-88ae-6e47e28a2a62 none            swap    sw              0       0
    # /home/niuniu/Movies was on /dev/sda8 during installation
    UUID=78B84D2CB84CE9E8 /home/niuniu/Movies           ntfs    defaults        0       2
    ```
