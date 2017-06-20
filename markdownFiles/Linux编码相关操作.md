# Linux编码相关操作

- 查看本地字符集
  ```Shell
  locale -a
  ```

- 查看所有支持的字符集
  ```Shell
  locale -m
  ```

- 将文件从gb2312转为utf8
  ```Shell
  iconv -f gb2312 -t utf8 input.txt -o output.txt
  ```

- 如果没有中文字符集，可以手动安装。
  - 安装中文包
  ```Shell
  yum -y groupinstall chinese-support 安装所有与中文支持相关的包
  ```
  - 修改字符编码配置文件
  ```Shell
  vi /etc/sysconfig/i18n
  ```

- 修改后内容如下
  ```Shell
  LANG="zh_CN.UTF-8"
  SUPPORTED="zh_CN:zh:en_US.UTF-8:en_US:en:zh_CN.GB18030"
  SYSFONT="latarcyrheb-sun16"
  ```

- 最后重启服务器：
  ```Shell
  reboot
  ```
