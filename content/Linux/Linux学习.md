+++
title = "Linux学习"
author = ["lqs_is_a_goodboy"]
description = "对Linux的学习的记录"
tags = ["Linux"]
categories = ["Linux"]
draft = false
+++

## Linux的目录结构 {#linux的目录结构}

-   **Linux 的目录结构遵循 FHS（Filesystem Hierarchy Standard，文件系统层次标准），不同文件夹有明确的用途分工。以下是常见目录的通俗解释：**
    -   核心目录
        -   /（根目录）：所有目录的起点，类似 Windows 的 C:\\。
    -   /bin（基本命令）
        -   存放所有用户都能用的 基础命令，如 ls、cp、rm 等。
        -   ❗ 系统启动或修复时必需的工具。
    -   /sbin（系统管理命令）
        -   存放需要 管理员权限 的命令，如 fdisk（磁盘分区）、ifconfig（网络配置）等。
    -   /etc（配置文件）
        -   系统和软件的 配置文件 集中地，例如：
            -   /etc/passwd（用户账户信息）
            -   /etc/nginx/（Nginx 配置）
            -   /etc/hosts（域名解析配置）。
    -   /usr（用户程序资源）
        -   存放 用户级软件和资源，类似 Windows 的 Program Files：
            -   /usr/bin：用户安装的命令（如 python、git）
            -   /usr/lib：软件依赖的库文件
            -   /usr/share：共享数据（文档、图标等）
            -   /usr/local：用户 手动编译安装 的软件（避免与系统包管理器冲突）。
    -   /var（动态数据）
        -   存放经常变化的文件：
            -   /var/log：系统日志（如 syslog、nginx/access.log）
            -   /var/www：网站文件（常见默认位置）
            -   /var/lib：数据库文件（如 MySQL 数据）。
    -   /home（用户家目录）
        -   每个用户的私人空间，存放个人文件、配置（如 ~/.bashrc）、下载等。
        -   ❗ Root 用户的家目录是 /root，而非 /home/root。
    -   /tmp（临时文件）
        -   所有用户都可写入的临时文件，系统重启时可能被清空。
    -   其他重要目录
        -   /boot（启动文件）
            -   内核文件、引导加载程序（如 GRUB）的配置。
        -   /dev（设备文件）
            -   硬件设备的抽象文件，如 /dev/sda（磁盘）、/dev/tty（终端）。
        -   /proc（内核和进程信息）
            -   虚拟目录，显示系统运行时的内核和进程状态（如 CPU、内存使用）。
        -   /opt（可选软件包）
            -   第三方大型软件（如 IDE、商业工具）的安装目录。
        -   /lib（系统库文件）
            -   /bin 和 /sbin 中命令依赖的库文件，如动态链接库（.so 文件）。
        -   /mnt 或 /media（挂载点）
            -   临时挂载外部存储设备（U盘、光盘等）。
    -   设计逻辑
        -   静态 vs 动态：
            -   /bin、/usr 存放静态文件（安装后不变）
            -   /var、/tmp 存放动态文件（运行时变化）。
    -   权限隔离：
        -   /bin、/sbin 区分普通用户和管理员命令。
    -   兼容性与扩展性：
        -   用户手动安装的软件建议放在 /usr/local 或 /opt，避免覆盖系统文件。
    -   举个栗子 🌰安装一个 Python 包：通过系统包管理器（如 apt）安装 → 文件分散到 /usr/bin、/usr/lib 等。手动编译安装 → 默认到 /usr/local/bin。下载解压版软件 → 可能直接放在 /opt 下。
