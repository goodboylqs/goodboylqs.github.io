+++
title = "org-mode嵌入html代码实现视频播放"
author = ["553912917@qq.com"]
description = "实现网页添加b站展示视频，原理就是得到b站播放器的html代码就嵌入到网页中，Hugo提供的shortcode功能是一个语法糖，本质上的原理也是一样的。"
date = 2023-12-23T00:00:00+08:00
lastmod = 2023-12-25T11:41:14+08:00
draft = false
featured_image = "../picture/狮王争霸.png"
+++

<span class="timestamp-wrapper"><span class="timestamp">&lt;2023-12-24 日 20:07&gt;</span></span>


## org-mode中嵌入html代码的方法 {#org-mode中嵌入html代码的方法}

```nil
\#+BEGIN_EXPORT HTML
HTML代码
\#+END_EXPORT
```


## org-mode嵌入b站播放器的html代码 {#org-mode嵌入b站播放器的html代码}

-   “这些死老外，为了赚钱，竟然弄些假药！”
    -   黄飞鸿：“欧美列强大老远跑到中国来，就是为了要捞他一笔，开口什么绅士风度，闭口自由民主，其实说来说去就是要骗钱，骗的好看一点，所以，我们只有自强、自重，才会有好日子过。”
-   黄飞鸿言：“李大人，正所谓胜者为王，败者为寇，现在，这个金牌在我黄某人手上，并非我赢了，大人为大显我民神威而办的这场狮王争霸赛，劳民伤财，死伤这么多人，其实，在世人眼里，我们都输了。以小民之见，我们不只要练武强身，以抗外敌，更重要的是广开言路，智武合一，那才是国富民强之道，区区一块牌子，能否改变国运，还望大人三思，这金牌，送给您做纪念吧。”

<!--listend-->

```nil
\#+BEGIN_EXPORT HTML
<iframe src="//player.bilibili.com/player.html?aid=98479429&bvid=BV1GE411c7sd&cid=168113370&p=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
\#+END_EXPORT
```


## 效果展示 {#效果展示}

<iframe width=400 height=300 src="//player.bilibili.com/player.html?aid=98479429&bvid=BV1GE411c7sd&cid=168113370&p=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>


## iframe标签的可选属性 {#iframe标签的可选属性}
