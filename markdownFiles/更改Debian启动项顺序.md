- 使用root用户登录,并编辑`/etc/default/grub`文件

- 修改文件中的`GRUB_DEFAULT=x`这一行,并将x改为你希望启动系统的序号.(假设启动项中第一个菜单为Debian,第二个为Windows,则Debian的序号为0,Windows为1,以此类推)

- 保存之后,执行命令`update-grub`,该命令会将刚才的修改反映到`/boot/grub/grub.cfg`文件里去,不然,重新启动后,上面所做的修改会丢失.
