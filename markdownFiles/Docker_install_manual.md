1. 获取安装脚本并执行
    
        wget -qO- https://get.docker.com/ | sh
    
2. 将用户加入docker组，以不使用sudo执行docker

        sudo usermod -aG docker user
        
3. 启动docker服务

        sudo service docker start
        
4. 配置国内镜像（网易）加速下载

    在 /etc/docker/daemon.json中增加以下配置，若daemon.json文件不存在需新建
    
    ```json
    {
        "registry-mirrors": ["http://hub-mirror.c.163.com"]
    }
    ```
5. 测试docker
    
        docker run hello-world
        
    该命令会安装hello-world镜像（若本地不存在则会从docker仓库自动下载）到容器，然后运行该容器后并退出。
    
