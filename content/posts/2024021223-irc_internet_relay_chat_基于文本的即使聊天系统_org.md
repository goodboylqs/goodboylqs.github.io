+++
title = "IRC（internet relay chat）互联网中继聊天使用教程（未完成）"
author = ["lqs_is_a_goodboy"]
description = "在emacs中使用irc（未完成）"
date = 2024-02-15T00:00:00+08:00
tags = ["编程", "irc", "emacs", "erc"]
categories = ["编程"]
draft = false
+++

tags
:


source
: <https://libera.chat/guides/basics>

<span class="timestamp-wrapper"><span class="timestamp">&lt;2024-02-12 周一 23:10&gt;</span></span>


## IRC基础知识 {#irc基础知识}

-   什么是IRC：internet relay chat（互联网中继聊天）， **irc是一种聊天协议**
-   IRC的工作原理：服务器地址是irc.libera.chat
-   IRC使用教程
    -   帮助
        -   如果你迷路了，可以加入#libera频道问问题，这就是这个频道存在的意义
    -   聊天
        -   聊天区既可以打字聊天，也可以向服务器或客户端发送命令， **命令以/开头**
    -   对话窗口
        -   切换到最顶上的Libera频道，你可以执行一些不想让别人看到的命令
        -   以#开头的就是频道
        -   频道下方的选项卡是您与网络上的人或机器人的私人对话
    -   私人对话
        -   可以通过双击对方的用户名开启私人对话
        -   也可以用/query命令，如 /query john hi!就是向用户名为john的用户发送消息hi!
    -   服务
        -   服务是帮助网络平稳运行的机器人，他们管理用户、频道等，使用服务可以注册用户名、登陆
    -   话题
        -   在大部分频道的顶端可以找到对频道话题的描述，如果内容展示不全，可以使用/topic命令来展示话题
    -   IRC命令备忘录
        -   /join #libera ：加入频道libera
        -   /part [频道名]（如[#libera]） 消息：离开xxx频道，如果不提供频道名就默认离开当前打字的频道
        -   /nick nickname：更改自己的昵称
        -   /msg nickname message：向用户名为nickname的用户发送消息，并且不开启一个新的对话框
        -   /query nickname [message] ：打开与用户nickname的信聊天框，并可以发送一条消息
            -   这个命令对于注册昵称很有用
            -   你的昵称是人们在libera.chat上识别你的方式，如果你注册了它，你可以多次使用同一个昵称；如果你没有注册，那么别的人也可以用你的昵称。 **一些频道要求你必须注册昵称才能发言**
            -   一个账户是你永久的识别信息，但是你的昵称只是账户当前的名字，识别的含义是登陆账户，NickServ表现得像个用户一样，但它是libera.chat提供的服务


## IRC使用（未完成） {#irc使用-未完成}


### 注册并识别一个新的账户的步骤 {#注册并识别一个新的账户的步骤}

-   第一步：注册账户：/nick 账户名
-   第二步：注册你的IRC 昵称：/msg NickServ REGISTER 密码 邮箱
    -   可以看做是向用户NickServ发送REGISTRE 密码 邮箱
    -   然后邮箱会收到一封信，可根据信中的指示完成注册


### 登陆账户 {#登陆账户}

-   **每次连接libera.chat服务器的时候你都要登陆你的账户**
    -   最简单的方式是使用<https://libera.chat/guides/sasl>
    -   或者你可以以 account:password 的格式为NickServ提供登陆细节： /connect irc.libera.chat 6667 账户名:密码
        -   如果你已经连接上了服务器，你可以手动验证： /msg NickServ IDENTIFY 账户名 密码


### 账户和昵称过期 {#账户和昵称过期}

-   注册的账户和昵称如果一段时间没有使用就会过期


### 连接irc.libera.chat {#连接irc-dot-libera-dot-chat}

-   **一些端口号，如下**

    | Plain-text | 6665-6667, 8000-8002 |
    |------------|----------------------|
    | TLS        | 6697, 7000, 7070     |
-
