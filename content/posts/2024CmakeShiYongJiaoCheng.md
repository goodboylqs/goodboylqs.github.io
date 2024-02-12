+++
title = "Cmake使用教程"
author = ["lqs_is_a_goodboy"]
description = "使用make和cmake工具自动构建项目"
date = 2024-02-12T08:00:00+08:00
lastmod = 2024-02-12T18:10:52+08:00
tags = ["编程", "cmake", "makefile", "make"]
categories = ["编程"]
draft = false
+++

tags
:


## Cmake使用教程 {#2024CmakeShiYongJiaoCheng}

-   Cmake是什么：Cmake是跨平台的构建工具
-   Cmake有什么用：可以通过编写简单的配置文件生成本地的Makefile，这样就不用亲自去编写Makefile了
    -   Makefile是什么：在Linux环境下使用make工具构建工程的时候需要makefile，makefile描述了整个工程的编译、连接规则。makefile是一个脚本，它定义了一系列的工程文件的编译规则，例如哪些文件先编译、哪些文件需要重新编译。makefile也可以执行操作系统的命令。
-   Cmake和Make的区别

    -   Make是一个工具，它是用于具体构建工程的，执行make命令一个项目的源代码就被编译、链接城二进制代码了，Make直接调用gcc等编译器。
    -   Cmake并不用于直接构建工程,Cmake用于生成构建工程所需要的makefile，makefile是make工作时必需的文件

    {{< figure src="/ox-hugo/2024-02-12_16-50-25_ee38eae9b8994a148f1f71c5f508895c.png.png" >}}


### make基础：编写基础的makefile {#make基础-编写基础的makefile}

-   makefile基础语法： **gcc前面是tab不是空格，一定要注意。也要注意编译命令的时间顺序，最后执行的命令在makefile最前面**
    ```make
    //    目标文件:依赖文件
    //        编译命令
          main:main.o fun0.o fun1.o fun2.o        //最后执行的命令在最前面
              gcc -o main.o fun0.o fun1.o fun2.o
          main.o:main.c
              gcc -c main.c
          fun0.o:fun0.c
              gcc -c fun0.c
          ……
    ```


### Cmake基础：编写基础的cmakelists.txt {#cmake基础-编写基础的cmakelists-dot-txt}

> 然后在main.c同级⽬录下编写CMakeLists.txt，内容如下：

```cmake
cmake_minimum_required (VERSION 2.8)
project (demo)
add_executable(main main.c)
```

-   **cmakelists.txt必需的内容：**
    -   cmake_minimum_required(VERSION xxx)
    -   project (项目名)
    -   add_executable(生成的可执行程序名 要编译的源文件1 2 3……)
-   备注
    -   **执行完cmake后要再执行make才能生成可执行程序的二进制代码，你需要找到cmake生成的makefile在哪个文件夹下，进入那个文件夹执行make**
    -   **如果要重新编译项目，要先执行make clean并删除cmake生成的makefile、cmakecache.txt等中间文件，避免之前的构建文件影响当前的构建**
    -   实际使用过程中，可以使用命令指定build path和source code path，build-path中会包含构建过程中生成的中间文件如makefile、cmakecache.txt等，这样可以方便的直接删除build-path下的中间文件
        ```shell
        cmake -B build-path -S source-path
        ```
