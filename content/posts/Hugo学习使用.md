+++
title = "Hugo使用学习"
author = ["lqs is a goodboy"]
date = 2023-12-14T08:00:00+08:00
lastmod = 2025-06-05T09:51:15+08:00
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


## 【极客日常】在hugo博客中利用shortcode嵌入bilibili视频 - 代码先锋网 {#JiKeRiChangZaihugoBoKeZhongLiYongshortcodeQianRubilibiliShiPinDaiMaXianFengWang}

原文链接：<https://codeleading.com/article/90653405942/>

hugo是当前热门的个人博客框架之一，和hexo同样是markdown文件跟博客帖子一一对应。有些同学想利用hugo制作个人视频博客，但发现在hugo博客中不支持直接在markdown里输入iframe标签，从而没有办法将其它网站的视频（比如b站）嵌进来。这种情况下，我们可以采用hugo内置的shortcode机制完成这个需求。我们参考的文章是create your own shortcodes，在这个文档中介绍了自定义shortcode从而快速渲染网页的方法，并且举了youtube以及vimeo的例子。照葫芦画瓢，我们也可以定义b站的shortcode。我们首先在 layouts/shortcodes 目录下创建 bilibili.html ，然后填充内容如下：

```html
<iframe
    src="//player.bilibili.com/player.html?bvid={{.Get 0 }}&page={{ if .Get 1 }}{{.Get 1}}{{ else }}1{{end}}"
    scrolling="no"
    height="768px"
    width="1024px"
    frameborder="no"
    framespacing="0"
    allowfullscreen="true"
    >
</iframe>
```

整个iframe是基于b站分享链接模板的。对于用户而言，主要关心bv号跟page（一个bv可能是个视频列表，有好几
part）。在 bilibili.html 中，定义了唯一的bv号作为query，而后page默认为1，用户可以自行指定。写好了 bilibili.html 中，在markdown里嵌入视频的话，用以下形式写就ok了：

{{&lt; bilibili BV1zZ4y1M7zN &gt;}}

```html
{{< bilibili BV1zZ4y1M7zN >}}
```

如果需要指定part，比如要看大逃脱第一季第3回，这样写就ok了：

{{&lt; bilibili BV1mD4y1U7z9 3 &gt;}}


### 方法二：直接使用bilibili的分享按钮获取页面内嵌的html代码然后放到自己的org file中 {#方法二-直接使用bilibili的分享按钮获取页面内嵌的html代码然后放到自己的org-file中}

-   **插入的视频是默认自动播放的，如果想要禁止自动播放，在插入的代码中的视频链接最后加上&amp;autoplay=0即可**
