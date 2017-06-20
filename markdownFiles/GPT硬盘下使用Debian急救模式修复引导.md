某天手贱，修改了Debian系统相关的配置文件，重启后直接进入grub而无法进入系统。

解决方法：

 - 使用Debian的ISO文件制作安装U盘。
 - 将制作好的U盘插入电脑USB并开机选择从U盘启动。
 - 进入Debian boot rescue模式。
 - 根据之前安装系统时的分区设置，选择在/dev/sda2上运行shell(/dev/sda2为我的根分区)。
 - 进入shell后执行以下命令：
   - 挂载根分区至/mnt
   ```Shell
   mount /dev/sda2 /mnt
   ```

   - 挂载boot分区至/mnt/boot
   ```Shell
   mount /dev/sda4 /mnt/boot
   ```
   - 挂载GPT硬盘的EFI引导信息至/mnt/boot/efi
   ```Shell
   mount /dev/sda1 /mnt/boot/efi
   ```
   -  挂载home分区至/mnt/home
   ```Shell
   mount /dev/sda5 /mnt/home
   ```
   - 重装GRUB
     - 生成GRUB引导
     ```Shell
     grub-mkconfig -o /boot/grub/grub.cfg
     ```   
     - 安装GRUB引导
   ```Shell
   grub2-install /boot/efi
   ```
- 重启系统
 ```Shell
 reboot
 ```

---
