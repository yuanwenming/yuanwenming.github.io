- 关闭RHEL7主板滴滴声
  ```bash
  echo "rmmod pcspkr" >> /etc/rc.d/rc.local
  chmod a+x /etc/rc.d/rc.local
  reboot 
  ```

- 配置RHEL7 telnet服务
  - 安装 telnet-server、xinetd
    ```bash
    yum install telnet-server -y
    yum install xinetd -y
    ```
    
  - 将xinetd、telnet服务加入开机自启动
  ```bash
  systemctl enable xinetd.service
  systemctl enable telnet.socket
  ```
  
  - 启动xinetd、telnet服务
  ```bash
  systemctl start telnet.socket
  systemctl start xinetd
  ```
  
  - 打开端口
  ```bash
  firewall-cmd --permanent --add-port=23/tcp 
  firewall-cmd --reload 
  ```
  
  - 修改/etc/xinetd.d/telnet 文件内容，配置连接数
    ```bash
    service telnet 
{ 
disable = no 
flags = REUSE 
socket_type = stream 
wait = no 
user = root 
server = /usr/sbin/in.telnetd 
log_on_failure += USERID 
instances = 60
} 
```

    - 重启服务
  ```bash
  service xinetd restart
  ```