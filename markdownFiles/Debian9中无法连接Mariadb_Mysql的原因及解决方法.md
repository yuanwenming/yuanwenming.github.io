### Debian9中无法连接MariaDb(Mysql)的原因及解决方法

---
今天安装好Debian9后，又继续安装了Mariadb（Mysql）数据库。

此时刚刚安装好数据库，root还没有设定密码，因此应该可以不用密码就应该可以登录，

但发现普通用户用mysql -uroot登录时无法登录，只能用Linux的root账户登录才可以。

百度后发现一篇帖子，和我的情况类似，参考后，顺利解决问题，在此记录，以供参考。

详细可参照[https://www.cnblogs.com/leolztang/p/5094930.html](https://www.cnblogs.com/leolztang/p/5094930.html)

<br>

下面是我的解决方法：

1. 使用Linux root登录，然后执行：mysql -uroot

2. 顺利登录后，切换到数据库mysql：use mysql;

3. 查看user表中，账号root的plugin，发现我的安装后默认内容为：unix_socket

4. 更改plugin内容：

	update user set plugin='mysql_native_password' where user='root';

5. 执行：flush privileges;

6. 重启数据库：systemctl restart mysql

重启后，即使使用Linux普通用户，也可顺利登录数据库。

---
