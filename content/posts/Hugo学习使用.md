+++
title = "Hugo使用学习"
author = ["lqs is a goodboy"]
description = "hugo的使用以及常见问题"
date = 2023-12-14T08:00:00+08:00
lastmod = 2025-06-05T10:22:59+08:00
tags = ["emacs", "hugo"]
categories = ["emacs", "hugo"]
draft = false
+++

tags
: emacs hugo


## hugo基本使用 {#hugo基本使用}

-   主页：content/_index.md

-   创建主页：hugo new –kind home \_index.md

-   章节：content/chapter/……………………

-   一个章节也是包含一个主页以及子章节创建章节log及主页：hugo new –kind chapter log/_index.md
    在章节log下面创建子章节first-day、second-day
    ```bash
    hugo new log/first-day/_index.md
    hugo new log/second-day/index.md
    hugo new log/third-day.md
    ```


## 为何插入了图片的org文件通过org-hugo专程.md文件并在网站部署后，图片无法正常显示？ {#为何插入了图片的org文件通过org-hugo专程-dot-md文件并在网站部署后-图片无法正常显示}

-   原因：在执行org-hugo转org文件为.md文件后，未执行hugo命令将存储在static/ox-hugo文件夹下的图片复制到public/ox-hugo文件夹中
