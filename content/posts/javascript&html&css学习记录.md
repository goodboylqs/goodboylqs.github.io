+++
title = "javascript&html&css学习记录"
author = ["lqs_is_a_goodboy"]
description = "从零开始学javascript，一个是为了使用wps的宏编程，一个是为了为自己的个人网站提供更高级的功能"
date = 2024-01-04T08:00:00+08:00
lastmod = 2024-01-14T17:35:11+08:00
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


#### html的div元素 {#html的div元素}

-   &lt;div&gt;标签定义html中的一个分割区块或者一个区域部分
-   **&lt;div&gt;标签常用于组合块级元素，以便通过CSS来对这些元素进行格式化**
    ```html
    <div style="color:#0000FF">
      <h3>这是一个在div元素中的标题</h3>
      <p>这是一个在div元素中的文本</p>
    </div>
    ```


## css {#css}


### css是什么 {#css是什么}

-   Cascading Style Sheets，层叠样式表


#### css的功能 {#css的功能}

-   对HTML的元素的样式进行描述， **实现了html的内容和表现的分离，可以极大提高工作效率**


### css语法：选择器（html标签）+声明（属性：值） {#css语法-选择器-html标签-plus-声明-属性-值}

-   声明：css声明总是以;结束，声明总是以{}括起来

<!--listend-->

```css
p
{
    color:red;   /*这是一个注释*/
    text-aligh:center;
}
```

-   css注释：以/\*开始，以\*/结束

<!--list-separator-->

-  CSS的id选择器和class选择器

    -   **如果想要在HTML元素中设置CSS样式，需要先在元素中设置id或class属性**

    <!--list-separator-->

    -  id选择器：为单一的html元素设置样式

        ```css
        /*id选择器可以为标有特定id的HTML元素指定特定的样式，HTML元素以id属性来设置id选择器，CSS中id选择器以#定义*/
        /*以下的样式规则应用于元素属性id="para1"*/
        #para1{
            text-aligh:center;
            color:yellow;
        }
        ```

    <!--list-separator-->

    -  class选择器：为一类html元素设置样式

        -   以下的规则适用于所有拥有center类的html元素

        <!--listend-->

        ```html
        <!--以下的规则适用于所有拥有center类的html元素-->
          <!DOCTYPE html>
          <html>
          <head>
          <meta charset="utf-8">
          <title>菜鸟教程(runoob.com)</title>
          <style>
          .center
          {
                  text-align:center;
          }
          </style>
          </head>

          <body>
          <h1 class="center">标题居中</h1>
          <p class="center">段落居中</p>
          </body>
          </html>
        ```

        -   你也可以指定特定的html元素如p元素使用class，以下的规则适用于所有的使用了class="center"的p元素
            ```html
            <!DOCTYPE html>
            <html>
            <head>
            <meta charset="utf-8">
            <title>菜鸟教程(runoob.com)</title>
            <style>
            p.center
            {
                    text-align:center;
            }
            </style>
            </head>

            <body>
            <h1 class="center">标题不受影响</h1>
            <p class="center">段落受影响，居中对齐</p>
            </body>
            </html>
            ```


### 创建css文件并引用的方法（外联、内联、内部） {#创建css文件并引用的方法-外联-内联-内部}


#### 外联样式表：在外部文件中写css代码，并 **在head中使用link标签来引用外部css。其中link标签的type、href属性要用到，type="text/css"，hrer="文件地址"** {#外联样式表-在外部文件中写css代码-并-在head中使用link标签来引用外部css-其中link标签的type-href属性要用到-type-text-css-hrer-文件地址}

```html
<head>
  <link rel="stylesheet" type="text/css" href="mystyle.css">
  </head>
```


#### 内部样式表：直接在head中用style标签写css代码 {#内部样式表-直接在head中用style标签写css代码}


#### 内联样式： **直接在相关的标签内使用style属性写css代码** {#内联样式-直接在相关的标签内使用style属性写css代码}

```html
<p style="color:sienna;margin-left:20px">这是一个段落</p>
```


#### 多重样式优先级：当一个元素在外部样式表、内部样式表、内联样式中都有css描述时，优先级为 **内联样式 &gt; 内部样式 &gt; 外部样式 &gt; 浏览器默认样式** {#多重样式优先级-当一个元素在外部样式表-内部样式表-内联样式中都有css描述时-优先级为-内联样式-内部样式-外部样式-浏览器默认样式}


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


### javascript的用法（内部的javascript和外部的js文件） {#javascript的用法-内部的javascript和外部的js文件}


#### 内部的javascript {#内部的javascript}

-   **在html页面中的javascript的代码必须在标签&lt;srcipt&gt;和&lt;/script&gt;之间**

<!--listend-->

```html
<!DOCTYPE html>
<html>
  <script>
    document.write("<h1>这是一个标题</h1>");
```

<!--list-separator-->

-  **可以通过javascript的函数来获取html的元素（如document.getElementById("demo")就是获取id="demo"的html元素），然后改变该元素的内容或样式**

    ```html
    <!DOCTYPE html>
    <html>
      <head>
        <script>
          function myfunc()
          {
              document.getElementById("demo").innerHTML="我的第一个javascript函数";
          }
          </script>
        </head>
        <body>
          <p id="demo">一个段落</p>
          <button type="button" onclick="myfunc()">尝试一下</button>
          </body>
      </html>
    ```


#### 外部的JavaScript {#外部的javascript}

-   **使用外部的.js文件，需要在&lt;script&gt;标签的src属性中设置该.js文件，外部脚本无需&lt;script&gt;标签**
    ```html
    <!DOCTYPE html>
    <html>
      <script src="myscript.js">
        </script>
      <body>
        <p id="demo">一个段落</p>
        </body>
      </html>
    ```


#### javascript的语法 {#javascript的语法}

<!--list-separator-->

-  javascript变量

    <!--list-separator-->

    -  javascript的变量类型

        <!--list-separator-->

        -  数值变量

            -   x = 5,y = 6;

        <!--list-separator-->

        -  表达式变量

            -   z = x+y;

        <!--list-separator-->

        -  文本值变量

            -   x = "goodboylqs"

    <!--list-separator-->

    -  声明javascript变量的方法

        -   **使用var关键字**
            var x = 3,y = "dog";
        -   **重新声明变量不会导致变量的值丢失**

        <!--listend-->

        ```js
        var car = "byd";
        var car;    //变量car的值依然是"byd"
        ```

        -   **2015年以前使用var声明js变量，2015年以后可以用let声明一个局部变量，const声明一个常量**
        -   **当声明新变量的时候，可以用new关键字来声明变量类型**
            ```js
            var carname = new String;
            var x = new Number;
            var cars = new Array;
            var person = new Object;
            ```

    <!--list-separator-->

    -  全局变量和局部变量

        -   **局部变量会在函数运行完以后被删除：可以在不同函数中使用名称相同的变量，只要函数运行完毕，本地变量就会被删除**
        -   **全局变量会在页面关闭后被删除：在函数外声明的变量是全局变量，网页上所有的脚本和函数都能访问它**

<!--list-separator-->

-  javascript的数据类型：值类型、对象类型

    <!--list-separator-->

    -  值类型

        -   字符串（string）、数字、布尔（true、false）、空

    <!--list-separator-->

    -  对象类型

        -   **对象（object）、数组（array）、函数（function）、正则（regexp）、日期（Date）**

        <!--list-separator-->

        -  **js数组**

            ```js
            //以下3钟方法皆可
            var cars = new Array();
            cars[0] = "saab";
            cars[1] = "volvo";
            cars[2] = "BMW";

            var cars = new Array（"saab","volvo","BMW"）;

            var cars = ["saab","volvo","BMW"];
            ```

        <!--list-separator-->

        -  js对象

            -   **javascript当中对象和函数也是变量**
            -   **JavaScript中几乎所有的事物对是对象**
            -   **就类似于c＋＋中的类，对象的属性以键值对的形式来定义**
                ```js
                //定义一个js对象
                var person = {

                firstname:"John",
                lastname:"Tom",
                id:556,
                fullName()

                };
                //对象属性寻址
                name = person.firstname;
                name = person["firstname"];

                //访问对象的方法属性
                name = person.fullName();
                ```

    <!--list-separator-->

    -  **动态类型：相同的变量可以作用不同的类型**

        ```js
        var x;
        var x = 5;  //x现在为数字
        var x = "John";  //现在x为字符串
        ```

<!--list-separator-->

-  javascript函数

    -   **javascript当中对象和函数也是变量**

    <!--list-separator-->

    -  定义javascript函数：使用function关键字

        ```js
          function myfunc(){
        }

        //有返回值的函数
        var myVar = myfunc(arg1,arg2);

        ```


### javascript的功能 {#javascript的功能}


#### javascript的4种输出数据的方式 {#javascript的4种输出数据的方式}

<!--list-separator-->

-  使用document.write()向HTML中写入

    <script>
      document.write("<h4>这是一个四级标题</h4>");
    </script>

    实际上用如下代码即可实现

    ```html
    <script>
      document.write("<h4>这是一个四级标题</h4>");
    </script>
    ```

<!--list-separator-->

-  使用window.alert()弹出警告框

    ```html
      <!DOCTYPE html>
    <html>
    <body>
    <h1>一个页面</h1>
    <p>一个段落</p>

    <script>window.alert(5 + 6);
    </script>

    </body>
    </html>
    ```

<!--list-separator-->

-  使用document.getElementById(id)来获取某个html元素并获取或插入内容

    ```html
    <!DOCTYPE html>
    <html>
      <h1 id="h1">一个页面</h1>
      <script>
        document.getElementById("h1").innerHTML= "已修改";
        </script>
      </html>
    ```

<!--list-separator-->

-  将javascript代码的执行结果写到控制台（方便调试）

    -   **在浏览器中使用F12调试，在调试窗口中点击console**

    <!--listend-->

    ```html
    <!DOCTYPE html>
    <html>
      <script>
        a = 5;
        b = 6;
        c = a + b;
        console.log(c);
        </script>
      <html>
    ```


#### javascript事件： **使用html元素的属性来触发事件** {#javascript事件-使用html元素的属性来触发事件}

-   **可以触发的事件类型：html页面加载完成时、input字段发生改变时、按钮点击时等**
-   语法
    ```html
    <html-element event="javascript代码">
    <button type="button" onclick="alert('欢迎')">点我</button>
    ```
-   **不推荐使用html元素中添加事件属性的方式：因为不符合编程高内聚、低耦合的原则**
    ```html
    <!->在html中关于元素的内容<-!>
    <button id="test"></button>
    ```

    ```js
    //在js文件中的js代码
    var test = document.getElementById("test");
    test.onclick = function changeContent(){
           ………………
    }
    //或者为html元素添加事件
    var test = document.getElementById("test");
    test.addEventListener("click",myfunc1);
    function myfunc1(){
    alert("你好");
    }
    ```


#### 改变html样式 {#改变html样式}

```js
x=document.getElementById("demo");  //查找元素
x.innerHTML="Hello JavaScript";    //改变样式
```
