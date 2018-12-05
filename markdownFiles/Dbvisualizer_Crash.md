Dbvisualizer是一个不错的数据库连接工具，基本上只要有JDBC驱动的数据库类型都可以使用它进行连接。

可以从官网[www.dbvis.com](https://www.dbvis.com)下载其社区免费版，虽然免费版也挺好用的，但毕竟功能有所阉割。

Pro版提供了更多的功能，比如右键菜单中的建表、更多的sql commander、及好用的结果集搜索栏等。

网上看到有破解方法可以对其进行破解，并注册为Pro版。

此文目的只是用于记录破解的过程，实验破解方法是否可行，强烈建议支持正版，尊重开发者的劳动成果。

    1. 找到Dbvisualizer安装目录中的lib
    2. 备份dbvis.jar
    3. 获取dbvis.jar中的com.onseven.dbvis.util.I中的A.class并反编译为A.java
    4. 修改A.java中的一个返回boolean类型的函数，直接将其返回值置为true
    5. 编译A.java,将得到的A.class在替换回dbvis.jar中
    6. 打开Dbvisualizer，在Helo->License Key中随便安装一个Pro key，具体问度娘要
