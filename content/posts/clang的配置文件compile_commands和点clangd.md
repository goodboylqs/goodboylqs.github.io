+++
title = "clang的compile_commands.json文件和.clangd文件"
author = ["lqs_is_a_goodboy"]
description = "clang编译器需要compile_commands.json来告诉编译器所需的上下文，现在可以使用cmake工具自动生成compile_commands.json"
date = 2024-02-11T00:00:00+08:00
lastmod = 2024-02-16T14:43:02+08:00
tags = ["编程", "cmake", "make", "clang", "clangd"]
categories = ["编程"]
draft = false
featured_image = "../picture/火之意志.jpg"
+++

## [编译器基础知识]({{< relref "编译器基础知识&emacs eglot配置#编译器基础知识" >}}) {#编译器基础知识}


## compile_commands.json {#compile-commands-dot-json}


### 什么是compile_commands.json：存储编译命令的数据库 {#什么是compile-commands-dot-json-存储编译命令的数据库}

-   参考 [**Compile command 和 clangd flags（很容易混淆）**]({{< relref "编译器基础知识&emacs eglot配置#compile-command-和-clangd-flags-很容易混淆" >}})
-   **要理解你的源代码，clangd需要知道你的构建标志（这只是C++中的一个事实，源文件不是独立的），而解释源代码需要一定量的上下文，例如你需要知道#include "stdio.h"中stdio.h文件的位置，c++编译器期望这些上下文通过command-line flags（命令行标志）来进行传递**
-   compile_commands.json就包含了这些上下文


### 一个compile_commands.json中都包含哪些内容 {#一个compile-commands-dot-json中都包含哪些内容}

-   **实际项目通常需要设置包含路径（使用 -I 标志）、定义预处理器符号、配置警告等。通常，编译数据库（也就是compile_commands.json）指定这些编译命令。**
-   clangd 在源文件的父文件中搜索compile_commands.json
-   由于 clangd 同时嵌入了 clang 的驱动程序（用于解释编译命令）和 clang 的解析器（由标志控制），因此可以传递给 clang 的大多数标志也将与 clangd 一起使用。 **其中最重要的flags如下：**
    -   设置#include的搜索路径：-I，-isystem，以及其他的
    -   控制使用的语言类型：-x,-std等
    -   预定义预处理器宏：-D等
    -   **没有这些flags，clangd经常会在解析源代码的时候失败，产生很多虚假的错误，例如#include 文件无法找到**


### compile_commands.json的格式 {#compile-commands-dot-json的格式}

> 编译数据库是一个JSON文件，它由一个 “命令对象command objects”数组组成，其中每个命令对象包含翻译单元的主文件、编译运行的工作目录和实际的编译命令。

```json
[
  { "directory": "/home/user/llvm/build",
    "command": "/usr/bin/clang++ -Irelative -DSOMEDEF=\"With spaces, quotes and \\-es.\" -c -o file.o file.cc",
    "file": "file.cc"},
...
]
```

> directory ：编译的工作目录。 command 或 file 字段中指定的所有路径必须是绝对路径或相对于此目录的路径。
> file ：此编译步骤处理的主要翻译单元源文件。这被工具用作编译数据库的键(key)。对于同一个文件可以有多个命令对象，例如，如果用
> 不同的配置编译同一个源文件。

<!--quoteend-->

> command ：执行编译命令。在JSON取消转义之后，这必须是一个有效的命令，以便在构建系统使用的环境中重新运行翻译单元的准确编译
> 步骤。参数使用shell引号和shell转义引号， ‘"’ 和 ‘\\’ 是惟一的特殊字符。不支持Shell扩展。
> arguments ：编译命令作为字符串列表执行。需要 arguments 或 command 。
> output ：此编译步骤创建的输出的名称。这个字段是可选的。它可以用来区分同一输入文件的不同处理模式。

-   **惯例是将compile_commands.json放到build文件夹下**


### 使用cmake/bear生成compile_commands.json {#使用cmake-bear生成compile-commands-dot-json}

-

-   **在执行cmake之前，一定要先写好CmakeLists.txt，格式如下**

<!--listend-->

```make
//以下3行是CmakeLists.txt的内容
cmake_minimum_required (VERSION 2.8)
project (demo)
add_executable(main main.c)
//将CmakeLists.txt放到build文件夹下，并执行cmake，会自动生成makefile等make编译所需的文件，最后再执行make


//最后在bash中执行如下命令生成compile_commands.json
cmake  -DCMAKE_EXPORT_COMPILE_COMMANDS=1    //使用cmake生成compile_commands.json
```


## .clangd:clangd的配置文件 {#dot-clangd-clangd的配置文件}

-   <https://clangd.llvm.org/config>
-   配置文件适用范围可以是项目也可以是整个系统
    -   适用于项目的配置文件：项目配置适用于其目录下的文件如proj/.clangd适用于proj/\*\*（ **clangd在源码文件的父目录中搜索.clangd** ）
-   语法
    -   If：在If下面的代码块中， **必须每个单独的条件都符合才能适用（但是对于同一个条件，如果有多个值，只需要符合其中一个值）**
        ```config
        IF:
          //如果是适用于项目的配置文件.clangd，那么路径使用相对路径；如果是适用于系统的配置文件，那么就是绝对路径
          // 所有的路径格式都应该是Unix风格的，即正斜杠
          PathMatch:.*\.h           //适用于当前目录下的所有.h文件
          PathExclude:include/llvm-c/.*   //不适用于include/llvm-c
        ```

        -   一个配置文件有多个 fragments（碎片），每个fragments可以适用于一个项目， **如果有多个If，可以用---来分隔不同的fragments**
    -   CompileFlags：影响源文件如何被解析
        ```config
        CompileFlags:                     # Tweak the parse settings
          Add: [-xc++, -Wall]             # treat all files as C++, enable more warnings
          Remove: -W*                     # strip all other warning-related flags
          Compiler: clang++               # Change argv[0] of compile flags to `clang++`
        ```

        -   Add、Remove不用说了，如果是有多个条件要添加删除，那么就可以写成如下格式
            ```config
             CompileFlags:
               Add:
            ​     - condition1
            ​     - condition2
            ​     - ……

            //一个例子，关于如何使用Remove
            Command: clang++ --include-directory=/usr/include -DFOO=42 foo.cc
            Configuration: Remove: [-I, -DFOO=*]
            Result: clang++ foo.cc
            ```
        -   Compiler一定要注意找到对应的可执行文件，我是用的绝对路径
    -   Compilationdatabase：规定compile_commands.json的搜索路径
        -   **如果.clangd中没有使用CompilationDatabase规定compile_commands.json文件的搜索路径，那么就只使用.clangd中的内容**
        -   **因为有的时候使用cmake自动生成的compile_commands.json有错误，不能正确的让clangd找到需要的文件，那么这个时候要么你手动去写compile_commands.json，要么就只用.clangd**
