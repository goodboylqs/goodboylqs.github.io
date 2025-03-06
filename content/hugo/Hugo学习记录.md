+++
title = "hugo学习记录"
author = ["lqs_is_a_goodboy"]
description = "使用hugo搭建网站并部署到github pages的基础知识、常见问题及解决"
tags = ["hugo"]
categories = ["hugo"]
draft = false
+++

## 问题 {#问题}

-   为什么使用org-hugo-to-md将org file输出为md file后在网页上不显示
    -   原因：没有在org file中加上title
-   如何在“遴选”目录页面中显示“遴选”目录下的所有子页面
    -   例子：在content/posts/_index.md 中添加shortcode{&lbrace;% children description="true" %&rbrace;}，就可以实现在_index.md 中显示posts下所有子页面的功能


## 使用 {#使用}


### 目录结构 {#目录结构}

-   hugo-theme-relearn一个页面对应public/posts下的一个文件夹
-   public/index.html对应于网站的主页
-   public/posts下的每一个文件夹对应于不同的页面
-   public/ox-hugo里面包含不同页面的图片
    -   /static/ox-hugo也是，是org-hugo-to-md后，ox-hugo将org file中的图片放到目录/static/ox-hugo下，然后执行hugo命令后，hugo会在构建网站时将/static/ox-hugo下的图片放到public/ox-hugo下


### 侧边栏 {#侧边栏}

-   如何生成一个新的侧边栏
    -   【使用hugo命令生成侧边栏文件夹】使用hugo new --kind chapter 菜单1/_index.md，会生成“content/菜单1”文件夹
    -   【将生成的.md文件移动到对应文件夹】将org-hugo-to-md生成的md文件移动到对应文件夹，例如将"少有人走的路.md"移动到“content/心理学”文件夹下
    -   【hugo构建网站】在根目录下使用hugo命令构建网站


### shortcodes：一种在.md文件中调用预定义好的html片段的方法 {#shortcodes-一种在-dot-md文件中调用预定义好的html片段的方法}

-   例子
    -   在content/posts/_index.md 中添加shortcode{&lbrace;% children description="true" %&rbrace;}，就可以实现在_index.md 中显示posts下所有子页面的功能
