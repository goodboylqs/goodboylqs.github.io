+++
title = "javascript学习记录"
author = ["lqs_is_a_goodboy"]
description = "从零开始学javascript，一个是为了使用wps的宏编程，一个是为了为自己的个人网站提供更高级的功能"
date = 2024-01-04T08:00:00+08:00
lastmod = 2024-01-05T10:59:37+08:00
tags = ["编程", "javascript"]
categories = ["编程"]
draft = false
+++

## 参考 {#参考}

-   [javascript教程](https://www.runoob.com/js/js-tutorial.html)
-   [Web 建站技术中，HTML、HTML5、XHTML、CSS、SQL、JavaScript、PHP、ASP.NET、Web Services 是什么？](https://www.zhihu.com/question/22689579/answer/22318058)


## 为什么要学习javascript:JavaScript 是 web 开发人员必须学习的 3 门语言中的一门 {#为什么要学习javascript-javascript-是-web-开发人员必须学习的-3-门语言中的一门}

-   HTML 定义了网页的内容
-   CSS 描述了网页的布局
-   JavaScript 控制了网页的行为


## javascript是什么 {#javascript是什么}

-   是一门脚本语言，是可插入html页面的编程代码，javascript代码的标准被所有浏览器支持，所以代码插入html后可由所有浏览器执行
-   有了表示内容和语义的 HTML，规定样式的 CSS，得到的是一个静态的页面，而javascript可以添加一些动态效果
    -   浏览器都会帮你实现一些 JS 可以用的工具（函数，对象什么的），你只要写一些 JS 的代码，保存在 xxx.js 里，在 html 文件中用 &lt;script&gt; 关联进来就可以用了


## javascript的功能 {#javascript的功能}


### 直接向HTML中写入 {#直接向html中写入}

<script>
  document.write("<h4>这是一个四级标题</h4>");
</script>

实际上用如下代码即可实现

```html
<script>
  document.write("<h4>这是一个四级标题</h4>");
</script>
```
