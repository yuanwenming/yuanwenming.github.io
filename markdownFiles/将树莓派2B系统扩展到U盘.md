step 1：首先使用树莓派官方系统img文件，将系统安装到TF卡
  
step 2：正常启动树莓派进入系统（默认用户：pi，默认密码：raspberry）

step 3：将U盘插入树莓派，并ls /dev查看，默认为sda

step 4：修改/etc/fstab文件，将/dev/mmcblk0p2修改为/dev/sda
`sudo vi /etc/fstab`

step 5：将/dev/mmcblk0p2内容拷贝到U盘
`sudo dd if=/dev/mmcblk0p2 /dev/sda`

step 6：拷贝完毕后，修改/etc/cmdline.txt，将root=/dev/mmcblk0p2 修改为 root=/dev/sda

step 7：reboot