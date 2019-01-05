系统强制断电后导致引导丢失及分区文件损坏，导致无法进入系统，以下是修复笔记，记录以备再次出现相同问题。

1. 使用Debian9启动盘，进入急救模式，使用`fdisk -l`查看磁盘分区情况，以我的电脑双盘双系统为例：
```
Disk /dev/sda: 2.7 TiB, 3000592982016 bytes, 5860533168 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: gpt
Disk identifier: C98AA7A7-3CA2-4964-8134-59C79F943D71

Device          Start        End    Sectors  Size Type
/dev/sda1        2048     206847     204800  100M EFI System
/dev/sda2      206848     468991     262144  128M Microsoft reserved
/dev/sda3      468992  210184191  209715200  100G Microsoft basic data
/dev/sda4   210184192  377956351  167772160   80G Microsoft basic data
/dev/sda5   377956352  692529151  314572800  150G Microsoft basic data
/dev/sda6   692529152 1111959551  419430400  200G Microsoft basic data
/dev/sda7  1111959552 1321674751  209715200  100G Microsoft basic data
/dev/sda8  1321674752 2370250751 1048576000  500G Microsoft basic data


Disk /dev/sdb: 232.9 GiB, 250059350016 bytes, 488397168 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: A1E33E9B-C772-4C37-B121-C8B120278008

Device         Start       End   Sectors   Size Type
/dev/sdb1       2048   1953791   1951744   953M EFI System   -> efi分区
/dev/sdb2    1953792 158203903 156250112  74.5G Linux filesystem   -> opt分区
/dev/sdb3  158203904 177735679  19531776   9.3G Linux swap
/dev/sdb4  177735680 275392511  97656832  46.6G Linux filesystem   -> root分区
/dev/sdb5  275392512 488396799 213004288 101.6G Linux filesystem   -> home分区
```

2. 使用以下命令挂在分区并重做引导,如果中间没有错误，会出现找到引导的日志，说明引导重建成功
```shell
mount /dev/sdb4 /mnt
mount /dev/sdb1 /mnt/boot/efi
mount /dev/sdb2 /mnt/opt
mount /dev/sdb5 /mnt/home
grub-mkconfig -o /mnt/boot/grub/grub.cfg
grub-install /mnt/boot/efi
```

3. 退出急救模式并重启系统，正常的话，已出现引导并可以进入

4. 由于强制断电同时也损坏了文件，导致启动时报错，报错信息类似“[FAILED] Failed to start File System Check on /dev/disk/by-uuid/xxxxxx”,但仍可进入文字界面，查看报错的uuid发现是/dev/sdb5分区，输入`fsck -f /dev/sdb5`，一路根据提示输入'y'就可以了，直至修复完成并重启，至此大功告成。

最后，还是尽量避免不要强制断电，切记！！
