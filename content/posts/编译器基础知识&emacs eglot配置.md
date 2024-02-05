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

<!--list-separator-->

-  compile_commands.json

    -   **解释源代码需要一定数量的上下文： 例如你需要知道#include "stdio.h" 中的stdio.h文件所在位置————————c++编译器期望这些上下文通过command-line flags（命令行标志）来进行传递————————命令的形式类似于clang -x objective-c++ -I/path/headers --target=x86_64-pc-linux-gnu -DNDEBUG foo.mm————————构建系统负责为每个文件收集正确的command-line flags（命令行标志）**
    -   **Compile command 和 clangd flags（很容易混淆）**
