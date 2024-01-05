+++
title = "javascript&html&css学习记录"
author = ["lqs_is_a_goodboy"]
description = "从零开始学javascript，一个是为了使用wps的宏编程，一个是为了为自己的个人网站提供更高级的功能"
date = 2024-01-04T08:00:00+08:00
lastmod = 2024-01-05T23:30:46+08:00
tags = ["编程", "javascript"]
categories = ["编程"]
draft = false
+++

-   **学习的目的不是为了学习，而是为了解决问题，所以遇到什么问题就去学相关的内容，时间经历有限不要啥都学**


## html {#html}

-   参考：<https://www.runoob.com/html/html-tutorial.html>


### html是什么 {#html是什么}

-   HyperText Markup Language，用于创建网页的一种标记语言，不是编程语言。 **html使用标签来描述网页**
-   **html运行在浏览器上，由浏览器来解析**


### html页面的结构 {#html页面的结构}

{{< figure src="/ox-hugo/2024-01-05_21-35-55_1704461747509.png" >}}

-   &lt;!DOCTYPE html&gt;声明为html5文档
-   &lt;html&gt;元素是HTML页面的根元素
-   **&lt;head&gt;元素包含了文档的元(meta)数据，如&lt;meta charset="utf-8"&gt;定义网页编码格式为utf-8**
-   &lt;title&gt;描述文档标题
-   **&lt;body&gt;包含了可见的页面内容，只有&lt;body&gt;&lt;/body&gt;区域才会在浏览器中显示**
-   &lt;p&gt;定义一个段落

{{< figure src="/ox-hugo/2024-01-05_21-40-24_1704462005161.png" >}}


### **html元素（包含一个开始标签到结束标签的所有内容）和html属性（属性是html元素提供的附加信息）** {#html元素-包含一个开始标签到结束标签的所有内容-和html属性-属性是html元素提供的附加信息}

-   **一个链接元素**
    ```html
    <a href="https://baidu.com">百度</a>
    ```

    -   上面从&lt;a&gt;开始到&lt;/a&gt;的内容就是一个html元素， **href是元素的属性，属性总是以键值对的形式出现，如name="value"**


#### 适用于大多数html元素的属性 {#适用于大多数html元素的属性}

| 属性  | 描述                            |
|-----|-------------------------------|
| class | 为html元素定义一个或多个类名， **类名从样式文件引入** |
| id    | 定义元素唯一的id                |
| style | 规定元素的行内样式              |
| title | 描述了元素的额外信息（作为工具条使用） |


#### **html常用元素的标签、属性** {#html常用元素的标签-属性}

-   [html标签列表（功能排序）](https://www.runoob.com/tags/ref-byfunc.html)

<!--list-separator-->

-  **html链接标签**

    -   标签&lt;a&gt;，属性href， **链接可以分文字链接、图片链接、内部链接、下载链接、id链接**

    <!--listend-->

    ```html
        <a href="https://baidu.com">百度</a>
    //图片链接
        <a href="https://baidu.com">
          <img src="wadasd.jpg" alt="示例图片">
          </a>
    //内部链接
        1.在想要链接到的地方创建锚点
        <a name="section2"></a>
        2.添加到锚点的链接
        <a href="#section2">跳转到section2</a>
    //下载链接
        <a href="document.pdf" download>下载文档</a>
    //id链接
        1.id属性可用于创建一个html书签，先创建一个书签，然后用链接链到这个书签
        <a id="tips">有用的提示部分</a>
        2.链接到书签
        <a href="https://page2.com/index.html#tips">访问有用的提示部分</a>
    ```


#### **html的head元素** {#html的head元素}

-   **&lt;head&gt;元素包含了所有的头部标签元素，在&lt;head&gt;元素中你可以插入脚本、插入样式文件、插入各种meta信息**
    -   可以在&lt;head&gt;元素区域添加的标签
        -   **&lt;meta&gt;:可添加html文档的描述、关键词、作者、字符集，meta中的数据可以用于浏览器搜索**
        -   &lt;base&gt;:为html文档中的所有链接标签提供了默认链接
            ```html
            <head>
            <base href="http://www.runoob.com/images/" target="_blank">
            </head>
            ```
        -   &lt;link&gt;:定义了文档与外部资源的关系，通常用于链接到样式表
        -   &lt;style&gt;:定义了html样式文件的引用地址，也可以在&lt;style&gt;中直接添加样式来渲染html文档


## css {#css}


## javascript {#javascript}


### 参考 {#参考}

-   [javascript教程](https://www.runoob.com/js/js-tutorial.html)
-   [Web 建站技术中，HTML、HTML5、XHTML、CSS、SQL、JavaScript、PHP、ASP.NET、Web Services 是什么？](https://www.zhihu.com/question/22689579/answer/22318058)


### 为什么要学习javascript:JavaScript 是 web 开发人员必须学习的 3 门语言中的一门 {#为什么要学习javascript-javascript-是-web-开发人员必须学习的-3-门语言中的一门}

-   HTML 定义了网页的内容
-   CSS 描述了网页的布局
-   JavaScript 控制了网页的行为


### javascript是什么 {#javascript是什么}

-   是一门脚本语言，是可插入html页面的编程代码，javascript代码的标准被所有浏览器支持，所以代码插入html后可由所有浏览器执行
-   有了表示内容和语义的 HTML，规定样式的 CSS，得到的是一个静态的页面，而javascript可以添加一些动态效果
    -   浏览器都会帮你实现一些 JS 可以用的工具（函数，对象什么的），你只要写一些 JS 的代码，保存在 xxx.js 里，在 html 文件中用 &lt;script&gt; 关联进来就可以用了


### javascript的功能 {#javascript的功能}


#### 直接向HTML中写入 {#直接向html中写入}

<script>
  document.write("<h4>这是一个四级标题</h4>");
</script>

实际上用如下代码即可实现

```html
<script>
  document.write("<h4>这是一个四级标题</h4>");
</script>
```
