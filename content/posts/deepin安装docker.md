+++
title = "docker基础+docker安装sqlserver、oracle"
author = ["553912917@qq.com"]
description = "在docker中安装sqlserver，下载sql的镜像，docker就自动配置好环境了，不用自己再一遍遍的配置了。"
date = 2023-09-04T18:18:00+08:00
tags = ["docker"]
categories = ["编程"]
draft = false
featured_image = "../picture/docker架构.png"
+++

## docker介绍 {#docker介绍}

-   docker是什么
    -   docker是一个开源的 **应用容器引擎**
-   docker能发挥什么作用———— **一次创建或配置，可以在任意地方正常运行。**
    -   **docker可以让开发者打包他们的应用和依赖包，到一个轻量级、可移植的容器中，然后发布到任何流行的Linux机器上，也可以实现虚拟化。**
        -   **Docker镜像中包含了运行环境和配置，所以Docker可以简化部署多种应用实例工作。比如Web应用、后台应用、数据库应用、大数据应用比如Hadoop集群、消息队列等等都可以打包成==一个镜像(类)==部署。**
        -   **Docker确保了执行环境的一致性，使得应用的迁移更加容易**
-   docker和虚拟机的区别

    {{< figure src="/ox-hugo/2023-09-05_15-56-01_screenshot.png" >}}
-   docker的组成
    -   **docker通过镜像（类）创建容器（实例）**
        -   Image（镜像）：相当于类
        -   Container（容器）：相当于实例
        -   Repository（仓库）：Docker存放Images的地方


## 安装docker {#安装docker}

-   <https://zhuanlan.zhihu.com/p/430652941>
-   删除旧版本docker
    sudo apt-get remove docker docker-engine docker.io containerd runc
    sudo rm -rf /var/lib/docker
    sudo rm -rf /var/lib/containerd
-   安装docker
    curl -fsSL <https://get.docker.com> | bash -s docker --mirror Aliyun
-   测试是否安装成功
    sudo systemctl start hello-world
    sudo docker run hello-world
-   配置加速器
    -   在 `/etc/docker/daemon.json` 中写入如下内容（如果文件不存在请新建该文件）
        ```nil
        #可以使用下面这个，比较快一些
        {
            "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn/","https://hub-mirror.c.163.com","https://registry.docker-cn.com"],
            "insecure-registries": ["10.0.0.12:5000"]
        }
        ```
        配置完成后需要重启Docker服务
        ```nil
        sudo systemctl daemon-reload
        # daemon-reload: 重新加载某个服务的配置文件，使修改的配置文件生效
        sudo systemctl restart docker

        docker info
        # 查看镜像地址与所配置的镜像地址是否匹配，如过匹配，则说明配置成功
        ```

        {{< figure src="/ox-hugo/2023-09-05_16-06-37_screenshot.png" >}}


## docker命令 {#docker命令}


### 镜像 {#镜像}

-   查看镜像
    -   sudo docker image ls -a
-   删除镜像
    -   sudo docker image rm imageid
-   重命名镜像
    -   `sudo docker image tag 镜像id 镜像名`


### 容器 {#容器}

-   查看docker容器
    -   docker container ls -a
    -   如果STATUS显示为UP，则说明映像正在容器中运行
-   重命名容器
    -   `sudo docker rename 容器id 容器名`
-   创建并启动容器：docker run
    -   注意该只能用于启动未创建的容器，如果启动已经创建的容器，用docker start
-   进入容器：docker exec -it 容器名 脚本名
    -   docker exec -it oracle11g bash
    -   **前面的docker介绍中已经用图示说明了docker包含要运行的应用程序(app)以及应用程序的运行环境(bin、lib)，因此当你进入容器后，实际上就是进入了包含app、bin、lib的环境**
        -   例如进入oracle的container，就包含要运行的oracle程序以及oracle的命令行sqlplus等辅助工具，在容器中运行sqlplus，连接oracle主程序，就可以在sqlplus中使用oracle的功能了


## docker安装sqlsrver {#docker安装sqlsrver}


### 下载sqlserver映像 {#下载sqlserver映像}

-   sudo docker pull mcr.microsoft.com/mssql/server:2017-latest


### 运行sqlserver映像 {#运行sqlserver映像}

```nil
- sudo docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=553912917@qq.com" \
   -p 1433:1433 --name sqlserver2017 --hostname sqlserver2017 \
   -d \
   mcr.microsoft.com/mssql/server:2017-latest
```

-   一定要注意 **sqlserver的密码一定要包含特殊字符，否则容器无法启动**

| 参数                                               | 说明                                                           |
|--------------------------------------------------|--------------------------------------------------------------|
| -e "ACCEPT_EULA=Y"                                 | 将 ACCEPT_EULA 变量设置为任意值，以确认接受最终用户许可协议。 SQL Server 映像的必需设置。 |
| -e "MSSQL_SA_PASSWORD=&lt;YourStrong@Passw0rd&gt;" | 指定至少包含 8 个字符且符合 SQL Server 密码要求的强密码。 SQL Server 映像的必需设置。 |
| -e "MSSQL_COLLATION=&lt;SQL_Server_collation&gt;"  | 指定自定义 SQL Server 排序规则，而不使用默认值 SQL_Latin1_General_CP1_CI_AS。 |
| -p 1433:1433                                       | 将主机环境中的 TCP 端口（第一个值）映射到容器中的 TCP 端口（第二个值）。 |
|                                                    | 在此示例中，SQL Server 侦听容器中的 TCP 1433，此容器端口随后会对主机上的 TCP 端口 1433 公开。 |
| --name sql1                                        | 为容器指定一个自定义名称，而不是使用随机生成的名称。 如果运行多个容器，则无法重复使用相同的名称。 |
| --hostname sql1                                    | 用于显式设置容器主机名。 如果未指定主机名，则主机名默认为容器 ID，这是随机生成的系统 GUID。 |
| -d                                                 | 在后台运行容器（守护程序）。                                   |
| mcr.microsoft.com/mssql/server:2017-latest         | SQL Server Linux 容器映像。                                    |

-   使用命令行连接sqlserver容器
    -   `sudo docker exec -it sqlserver2017 bash`
-   在容器内使用sqlcmd连接sqlserver
    -   先创建软链接 `ln -s /opt/mssql-tools/bin/sqlcmd /usr/bin/sqlcmd`
    -   `/opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "553912917@qq.com"`
        -   -S：[-S server or Dsn if -D is provided]
        -   -U：[-U login id]
        -   -P：[-P password]


## docker安装oracle {#docker安装oracle}

-   查看已经安装的images

    {{< figure src="/ox-hugo/2023-09-05_16-54-14_screenshot.png" >}}

    -   发现其中有名为 `oracle_11g` 的image，我们的目标就是以这个image为蓝本，生成一个container
-   第一次创建并启动container用docker run
    -   `sudo docker run -d -it -p 1521:1521 --name oracle_11g --restart=always oracle_11g`
        -   -p：指定端口号
        -   --name：指定生成的container的name
        -   -d：Run container in background and print container ID
        -   -it：Allocate a pseudo-TTY、Keep STDIN open even if not attached
    -   阿里的镜像密码都是helowin
    -   **进入容器后要执行su root命令切换为管理员权限才能修改/etc/profile，这个/etc/profile不是你本机的/etc/profile而是oracle镜像的/etc/profile**
        -   `vi /etc/profile`
            把下面的内容加入到文件中
            ```nil
            export ORACLE_HOME=/home/oracle/app/oracle/product/11.2.0/dbhome_2
            export ORACLE_SID=helowin
            export PATH=$ORACLE_HOME/bin:$PATH
            ```
            保存后执行 `source /etc/profile` 加载环境变量
