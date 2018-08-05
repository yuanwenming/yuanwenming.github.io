# Debian本地安装deb文件

1. 下载希望安装的deb安装包
2. 使用`sudo dpkg -i xxx.deb`安装deb包
3. 安装过程中，可能会因为缺少依赖而安装失败，如果失败，执行`sudo apt --fix-broken install`安装缺少依赖后会自动安装第2步中的deb文件
