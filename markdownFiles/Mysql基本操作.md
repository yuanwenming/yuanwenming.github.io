# Mysql基本操作

---

### 1. 安装Mysql


在Debian系中，可以使用如下命令安装Mysql：

```bash
sudo apt-get install mysql-server mysql-client
```

在RedHat系中，可以使用如下命令安装Mysql：

```bash
sudo yum install mysql-server mysql-client
```

mysql-server是服务器程序,mysql-client是客户端程序。我们可通过客户端程序来管
理服务器,也可通过一些开源 的GUI程序来维护服务器,如phpmyadmin,mysqlcc
等。推荐使用phpmyadmin这个B/S的管理程序,通过浏览器就可方便高效地管 理网
络上的数据库。

### 2. 登录Mysql

登录MySQL的命令是mysql, mysql 的使用语法如下:
`mysql [-u username] [-h host] [-p[password]] [dbname]`
username与password分别是MySQL的用户名与密码,mysql的初始管理帐号是root,没有密码,注意:这个root用户不是Linux的系统用户。MySQL默认用户是root,由于初始没有密码,第一次进时只需键入mysql即可。

    [root@test1 local]# mysql
    Welcome to the MySQL monitor.
    Commands end with ; or \g.
    Your MySQL connection id is 1 to server version: 4.0.16-standard
    Type 'help;' or '\h' for help. Type '\c' to clear the buffer.
    mysql>

出现了“mysql>”提示符,恭喜你,安装成功!
增加了密码后的登录格式如下:

    Enter password: (输入密码)

其中-u后跟的是用户名,-p要求输入密码,回车后在输入密码处输入密码。
注意:这个mysql文件在/usr/bin目录下,与后面讲的启动文件/etc/init.d/mysql不是一个文件。

### 3. Mysql比较重要的目录
MySQL安装完成后不像SQL Server默认安装在一个目录,它的数据库文件、配置文件和命令文件分别在不同的目录,了解这些目录非常重要,尤其对于linux的初学者,因为Linux本身的目录结构就比较复杂,如果搞不清楚MySQL的安装目录那就无从谈起深入学习。
下面就介绍一下这几个目录。

1、数据库目录
/var/lib/mysql/

2、配置文件
/usr/share/mysql(mysql.server命令及配置文件)

3、相关命令
/usr/bin(mysqladmin mysqldump等命令)

4、启动脚本
/etc/rc.d/init.d/(启动脚本文件mysql的目录)

### 4. 修改登录密码
MySQL默认没有密码,安装完毕增加密码的重要性是不言而喻的。

1、命令
`usr/bin/mysqladmin -u root password 'new-password'`
格式:mysqladmin -u用户名 -p旧密码 password 新密码

2、例子
例1:给root加个密码123456。
键入以下命令 :

    [root@test1 local]# /usr/bin/mysqladmin -u root password 123456
注:因为开始时root没有密码,所以-p旧密码一项就可以省略了。

3、测试是否修改成功

1)不用密码登录

    [root@test1 local]# mysql
    ERROR 1045: Access denied for user: 'root@localhost' (Using password: NO)
显示错误,说明密码已经修改。

2)用修改后的密码登录

    [root@test1 local]# mysql -u root -p
    Enter password: (输入修改后的密码123456)
    Welcome to the MySQL monitor.
    Commands end with ; or \g.
    Your MySQL connection id is 4 to server version: 4.0.16-standard
    Type 'help;' or '\h' for help. Type '\c' to clear the buffer.
    mysql>
成功!
这是通过mysqladmin命令修改口令,也可通过修改库来更改口令。

### 5. 启动与停止

- 启动
`sudo service mysql start`
- 停止
`sudo service mysql stop`

### 6. 常用基本操作

- 显示数据库
`show databases;`

- 显示数据库中的表
`show tables;`

- 显示表的结构
`describe 表名;`

- 显示表中的记录
`select * from 表名;`

- 建库
`create database 数据库名;`

- 建表
```sql
use 库名;
create table 表名(字段定义列表);
```

- 增加记录
`insert into 表名 values();`

- 修改记录
`update 表名 set 字段=值;`

- 删除记录
`delete from 表名;`

- 删库和删表
`drop database 库名;`
`drop table 表名`

### 7. 增加数据库用户
`grant all privileges on 数据库名.表名 to 用户@登录主机 indentified by '密码';`


### 8. 备份与恢复
- 备份
`mysqldump -u root -p --opt 库名 > 文件名`
- 恢复
`mysqldump -u root -p 库名 < 文件名`
