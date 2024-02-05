+++
author = ["lqs_is_a_goodboy"]
draft = false
+++

## 编译器基础知识 {#编译器基础知识}


### compiler 和 parser 的关系 {#compiler-和-parser-的关系}

-   compiler包含parser


### 编译器一般构成 {#编译器一般构成}

-   传统的编译器通常分为三个部分——————前端（frontend）、优化器（Optimizer）、后端（backend）
    -   前端：前端主要负责词法和语法分析，将源代码转化为抽象语法树
    -   优化器：对前端得到的代码进行优化
    -   后端：已经优化的中间代码转化为针对各自平台的机器码


### gcc 和 llvm 和 clang {#gcc-和-llvm-和-clang}

-   gcc————GNU compiler collection（GNU 编译工具集合），是一套由GNU开发的编程语言编译器，可处理C、C＋＋、Java等语言
-   llvm————Low level virtual machine（底层虚拟机）。简而言之，可以作为 **多种编译器的后台** 来使用
-   **clang 是 llvm的前端** ，可以用来编译支持C,C++,objective-c语言
    -   **clang 是以 llvm为后端的高效易用的前端**


## eglot配置 {#eglot配置}

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
