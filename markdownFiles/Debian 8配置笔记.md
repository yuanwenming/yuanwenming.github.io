> 以下操作均以root用户操作，进行下列配置以前记得切换到root用户。

### 1. **添加163软件源**

切换到root用户，执行`vi  /etc/apt/sources.list`，在sources.list文件中添加以下内容
```Shell
deb http://mirrors.163.com/debian jessie main non-free contrib
deb http://mirrors.163.com/debian jessie-proposed-updates main contrib non-free
deb http://mirrors.163.com/debian-security jessie/updates main contrib non-free   
deb-src http://mirrors.163.com/debian jessie main non-free contrib
deb-src http://mirrors.163.com/debian jessie-proposed-updates main contrib non-free
deb-src http://mirrors.163.com/debian-security jessie/updates main contrib non-free  
deb http://http.us.debian.org/debian jessie main contrib non-free
deb http://non-us.debian.org/debian-non-US jessie/non-US main contrib non-free
deb http://security.debian.org jessie/updates main contrib non-free
```
### 2. **更新系统**
```Shell
apt-get update
apt-get upgrade
```
### 3. **安装sudo使普通用户有系统管理的权利**
```Shell
apt-get install sudo	(安装sudo)
chmod +w /etc/sudoers   (添加write权限)
vi etc/sudoers          (编辑sudoers文件，并在root后面把普通用户添加进去并保存退出)
chmod 400 /etc/sudoers  (将sudoers权限改为root只读)
exit                    (退到普通账户)
```
### 4. **安装文泉译字体**
```Shell
apt-get install ttf-wqy-zenhei ttf-wqy-microhei synaptic
```
### 5. **添加中文字符集支持**
`dpkg-reconfigure locales`

### 6. **安装windows字体**
   1. 在`usr/share/fonts/`中简历目录winfonts<p>
   2. 将windows字体拷贝到`/usr/share/fonts/winfonts`中<p>
   3. 运行以下命令<p>

  ```Shell
  mkfontscale
  mkfontdir
  fc-cache
  ```
