+++
title = "编译器基础知识&clang compile_commands.json"
author = ["lqs_is_a_goodboy"]
description = "在emacs使用clangd作为语法分析的后台，但是配置过程中存在一些问题，主要是对clang以及编译器的知识不了解，把相关学习内容记录一下。"
date = 2024-02-03T00:10:00+08:00
lastmod = 2024-02-12T18:18:34+08:00
tags = ["编程", "emacs", "clang", "lsp", "clangd"]
categories = ["编程"]
draft = false
+++

## 编译器基础知识 {#编译器基础知识}


### 编译器一般构成 {#编译器一般构成}

-   传统的编译器通常分为三个部分——————前端（frontend）、优化器（Optimizer）、后端（backend）
    -   前端：检察源码、解析错误、语法分析、语义分析， **构建于一个特定于语言的抽象语法树AST**
    -   优化器：优化前端生成的AST
    -   后端：将代码映射到目标指令集，还负责生成利用所支持的体系结构的不寻常特性的良好代码
-   这种前端＋优化器＋后端的设计带来的好处： **支持多种语言作为source code，支持多种目标架构如X86,powerpc，arm**

    -   **比如可以使用c语言作为源码输入，选择ARM的代码生成器作为后端，如果想使用一种新的编程语言，无需改变优化器和代码生成器，只需要编写该语言的前端部分即可（我的理解：可以把优化器和后端看作一个整体，这个整体接受的代码格式是不变的，可以叫他格式A，前端对不同类型的语言都要生成格式A的代码，这样后端就不用考虑前端）**

    {{< figure src="/ox-hugo/2024-02-12_10-18-35_d113e20757f87d818690097df558f60f.png" >}}


### gcc 和 llvm 和 clang 和 clangd {#gcc-和-llvm-和-clang-和-clangd}

{{< figure src="/ox-hugo/2024-02-12_10-22-31_2b3f2357317965abbaddb6f05a548049.png" >}}

-   llvm：Low level virtual machine（底层虚拟机）。简而言之，可以作为 **多种编译器的后台** 来使用
    -   广义的llvm：包含前端、优化器、后端在内的compiler
    -   狭义的llvm：编译器的优化器和后端部分
-   clang：编译器的前端

    {{< figure src="/ox-hugo/2024-02-12_10-23-24_d23fb6324245acc1a188514cdf742432.jpeg" >}}
-   gcc：GNU compiler collection（GNU 编译工具集合），是一套由GNU开发的编程语言编译器，可处理C、C＋＋、Java等语言
-   clangd：一个语言服务器，向您的编辑器添加智能功能：代码完成、编译错误、去定义等


## compile_commands.json（官方文档翻译摘抄） {#compile-commands-dot-json-官方文档翻译摘抄}

-   clangd:clang是一个工具集合，包含众多工具，clangd是其中一个
-   <https://clangd.llvm.org/installation.html#project-setup>
-   [clangd compile commands](https://clangd.llvm.org/design/compile-commands)


### 碰到丢失#include文件等错误信息的原因和解决方法 {#碰到丢失-include文件等错误信息的原因和解决方法}


#### 原因 {#原因}

> To understand your source code, clangd needs to know your build flags. (This is just a fact of life in C++, source files are not self-contained).
>
> By default, clangd will assume your code is built as clang some_file.cc, and you'll probably get spurious errors about missing #included files, etc. There are a couple of ways to fix this.

要理解你的源代码，clangd 需要知道你的构建标志。（这只是 C++ 中的一个事实，源文件不是独立的）。

默认情况下，clangd 会假定您的代码是作为 clang some_file.cc 构建的，您可能会收到有关丢失 #included文件等的虚假错误。有几种方法可以解决此问题。


#### 解决方法 {#解决方法}

-   **解释源代码需要一定数量的上下文： 例如你需要知道#include "stdio.h" 中的stdio.h文件所在位置————————c++编译器期望这些上下文通过command-line flags（命令行标志）来进行传递————————命令的形式类似于clang -x objective-c++ -I/path/headers --target=x86_64-pc-linux-gnu -DNDEBUG foo.mm————————构建系统负责为每个文件收集正确的command-line flags（命令行标志）**

<!--list-separator-->

-  **Compile command 和 clangd flags（很容易混淆）**

    -   clangd flags: 当编辑器启动的时候，clangd flags就会被传递到编辑器了。它们会在clangd 的日志非常靠前的部分显示
    -   compile command（或者说 compile flags）:编译命令（或编译标志）是在clangd中构造和解释的虚拟命令。当文件打开时，它会被记录下来。

<!--list-separator-->

-  一个compile command中都有什么

    -   clangd 模拟 clang 解释文件的方式。默认情况下，它的行为大致类似于 clang $FILENAME， **但实际项目通常需要设置包含路径（使用 -I 标志）、定义预处理器符号、配置警告等。通常，编译数据库指定这些编译命令。clangd 在源文件的父文件中搜索compile_commands.json。**
    -   由于 clangd 同时嵌入了 clang 的驱动程序（用于解释编译命令）和 clang 的解析器（由标志控制），因此可以传递给 clang 的大多数标志也将与 clangd 一起使用。其中最重要的flags如下：
        -   设置#include的搜索路径：-I，-isystem，以及其他的
        -   控制使用的语言类型：-x,-std等
        -   预定义预处理器宏：-D等
        -   **没有这些flags，clangd经常会在解析源代码的时候失败，产生很多虚假的错误，例如#include 文件无法找到**
    -   和flags一样，命名程序argv[0]也会影响编译器的行为，clang++会将原码作为C++源码进行解析，而clang则会认为源码是C源码

<!--list-separator-->

-  compile commands从哪来

    -   通常，clangd 首先从编译数据库（compilation databases）中获取基本的编译命令，然后对其进行一些调整
    -   compilation databases描述了代码库的编译命令，他可以是
        -   一个名为compile_commands.json文件，该文件为每一个源码文件列举了命令，通常使用CMake作为构建系统
        -   一个名为compile_flags.txt的文件，为每一个源码文件列举了flags，通常用于简单项目手动编写。
        -   我们在源码文件夹、源码文件夹的父文件夹中检查有没有上述的两个文件
    -   用于头文件的命令

<!--list-separator-->

-  如何设置compile flags

    -   Add：追加编译选项
    -   Remove：减少编译选项
    -   Complier：选择使用的编译器，如clang或者clang++
    -   complie_flags.txt内容
        ```compile_flags.txt
        CompileFlags:                   # Tweak the parse settings
          Add: [-xc++, -Wall]             # treat all files as C++, enable more warnings
          Remove: -W*                     # strip all other warning-related flags
          Compiler: clang++               # Change argv[0] of compile flags to `clang++`

        -----------------------------------------上面那个不好用用下面的------------------------
        -xc
        -I
        C:\msys64\mingw64\include    //-xc表示语言是c语言，-I需要换行，第三行指定库文件的位置
        ```
