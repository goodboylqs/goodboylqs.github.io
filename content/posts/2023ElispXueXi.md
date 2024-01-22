+++
title = "Elisp学习"
author = ["553912917@qq.com"]
description = "elisp基础语法"
date = 2023-11-15T23:43:00+08:00
lastmod = 2024-01-14T10:44:18+08:00
tags = ["elisp学习", "emacs"]
categories = ["编程"]
draft = false
+++

tags
:


## Elisp学习 {#2023ElispXueXi}

filetags: @编程 elisp学习 emacs

tags
: 编程 emacs

source
: <http://smacs.github.io/elisp/>

date
: <span class="timestamp-wrapper"><span class="timestamp">&lt;2023-11-15 周三 23:41&gt;</span></span>


### elisp的运行环境 {#elisp的运行环境}

-   只有在lisp-interaction-mode下，才能执行elisp代码
    -   M-x lisp-interaction-mode
    -   在行尾右括号后面按C-j
-   elisp不好作为可执行方式来来运行，所有的elisp都是在emacs的环境下


### 在org-mode中插入src代码块 {#在org-mode中插入src代码块}

-   C-c C-,(org-insert-structure-template)


### 在org-mode中的src区域运行代码 {#在org-mode中的src区域运行代码}

-   将光标移动到src区域，按C-c C-c即可运行代码
-   将光标移动到src区域，按C-c '可对代码进行编辑


### elisp表达式 {#elisp表达式}

-   elisp的表达式是一个括号括起来，成为一个S-表达式
-   执行一个S-表达式的方法有2个
    -   用C-j
    -   用C-x C-e————C-x C-e是一个全局按键，几乎在任何地方都可以使用。也就是说哪怕是没有打开lisp-interaction-mode的情况下使用C-x C-e也可以运行代码，运行结果会在minibuffer中显示
-   **elisp表达式的作用 ≠ elisp表达式的返回值**
    -   (message "hello world")中message函数的作用是在minibuffer中显示一个字符串，但它的返回值是"hello world"

<!--listend-->

```elisp
(message "hello world")
```


### elisp的函数和变量 {#elisp的函数和变量}


#### elisp函数 {#elisp函数}

-   定义一个函数
    ```elisp
    ;;    (defun 函数名(参数列表)
    ;;      函数文档  //可省略 *如果添加了函数文档，那么使用C-h f查看该函数的时候会显示函数文档的内容*
    ;;      函数体)
    (defun helloworld(name)
      "对name说hello"
      (message "hello,%s" name))
    (helloworld "emacser")
    ```
-   **elisp中的函数是全局的，变量也很容易成为全局变量（因为全局变量和局部变量的赋值都是使用setq），所以名字互相不冲突是很关键的**


#### elisp变量 {#elisp变量}

-   elisp变量无需像c语言变量一样声明，可以直接赋值

    -   **也就是说elisp变量没有类型（存疑）**

    <!--listend-->

    ```elisp
    (setq foo "I'm, foo")
    (message foo)
    ```

<!--list-separator-->

-  使用defvar声明变量

    ```elisp
    ;; (defvar 变量名 变量值
    ;;   变量文档)      //可省略 *如果添加了变量文档，使用C-h v查看该变量的时候会显示变量文档的内容*
    (defvar foo "Did I have a value?"
      "A demo variable")
    foo
    ```

    -   **defvar 与 setq的区别**
        -   defvar可以为变量提供文档
        -   de

<!--list-separator-->

-  全局变量和局部变量

    -   **elisp中的函数是全局的，变量也很容易成为全局变量（因为全局变量和局部变量的赋值都是使用setq），所以名字互相不冲突是很关键的**
        -   所以局部变量很重要
    -   elisp里可以用let和let\* 进行局部变量的绑定

        -   let的语法
            ```elisp
            (let (bindings)        ;;bindings可以使(var value)这样对var赋初始值的形式，也可以是直接var声明一个初始值为nil的变量
              body)
            ```
        -   eg1.我写的，有错误，错在①没有使用setq对变量进行赋值②变量乘法的表达式不对③对let的用法，bindings部分没有使用括号括起来

        <!--listend-->

        ```elisp
        (defun circle-area(radix)
          (let pi 3.14)
          (let area pi*radix*radix)
          (message "radix is %.2f,circle's area is %.2f" radix area))
        (circle-area 2)  ;;提示wrong type argument,listp 3.1415
        ```

        -   eg2.有错误: **let中不能再包含let声明，对变量area的声明利用第一个let进行即可。而且对let声明的变量的使用，一定要再let的body部分，不能在let的括号外**
            ```elisp
            (defun circle-area(radix)
              (let (pi 3.14)    ;;bindings为(var value)的形式
                (let  area)          ;;bindings为(var)的形式，var的初始值为nil
                (setq area (* pi radix radix))
                (message "直径为%.2f的圆面积是%.2f" radix area))
              )
            (circle-area 3)  ;;提示wrong type argument,listp 3.1415
            ```
        -   eg3.有错误：对let的使用方式错误，bindings部分没有使用括号括起来
            ```elisp
            (defun circle-area(radix)
              (let (pi 3.1415926) area)   ;正确的应该是(let( (pi 3.14) area) )，(pi 3.14) area是bindings部分，body部分是空的
              (setq area (* pi radix radix))
              (message "直径为%.2f的圆面积是％.2f" radix area))
            (circle-area 3)          ;;提示wrong type argument,listp 3.1415
            ```
        -   eg.4.正确的
            ```elisp
            (defun circle-area(radix)
              (let ((pi 3.1415926) area))    ;;一个完整的(let(bindings)body)
              (setq area (* pi radix radix))  ;;和前面的let声明没有任何关系，只是用了let声明的变量
              (message "直径为%.2f的圆面积是%.2f" radix area)
              )
            (circle-area 3)
            ```


#### lambda表达式 {#lambda表达式}

-   可以把lambda表达式当成一个值，lambda表达式最常用的就是作为参数传递给其他函数
-   **lambda表达式的形式和defun完全一样**
    ```elisp
    (lambda (arguments-list)
      "函数文档"
      body)
    ```
-   调用lambda表达式与调用defun定义的函数的区别： **用lambda定义的函数没有函数名，用defun定义的函数有函数名，所以用defun定义的函数直接用函数名调用即可，而用lambda定义的函数名需要用funcall调用**
    ```elisp
    (funcall (lambda (name)
               (message "Hello,%s!" name))
             "Emacser"
             ) ;; *把lambda函数看作一个整体A，A就是一个函数，实际上就是（funcall A 参数），普通的函数调用是（A 参数）*
    ```
-   可以把lambda表达式赋值给一个变量
    ```elisp
    (setq foo (lambda (name)
                (message "Hello,%s!" name))
          )
    (funcall foo "Emacser")
    ```


### 控制结构 {#控制结构}


#### 顺序执行 {#顺序执行}

```elisp
;; (progn A B C...)  ;;让ABC顺序执行
(progn
  (setq foo 3)
  (message "Square of %d is %d" foo (* foo foo)))
```


#### 条件判断 {#条件判断}

-   **elisp有两个最基本的条件判断表达式if 和 cond**

<!--listend-->

```elisp
(if condition
    then
  else)
;; 注意if后面执行的语句如果超过1句，一定要用progn，而不是直接括号括起来
;; 注意是(if t)而不是(if (t))，后者是错误的，例如(if (numberp 1))是正确的

(cond
 (case1 do-when-case1)
 (case2 do-when-case2)


 (t do-when-none-meet))
```


#### 循环 {#循环}

```elisp
(while condition
    body)


(while (> n 1)
    (setq n = n - 1))

```


### elisp数据类型 {#elisp数据类型}

-   **elisp里的对象都是有类型的，而且你得到一个变量之后可以用很多方法来检测这个变量的类型**


#### 数字 {#数字}

-   emacs数字有整数和浮点数
-   \*整数类型的检测函数intergerp、浮点数类型的检测函数floatp、数字类型的检测函数numberp

<!--listend-->

```elisp
;; (message "%s" (numberp 1))
(if (numberp 1)    ;; *注意是(if t)而不是(if (t))，后者是错误的*
    ;; *elisp的if执行的语句如果超过1句一定要用progn，而不是直接括号括起来*
    ;; ((message "t"))    ;;错误写法
    (progn
      (message "(numberp 1) is t"))    ;;正确下发
  (message "(numberp 1) is f"))
```


### elisp常用函数 {#elisp常用函数}


#### creating buffers {#creating-buffers}

<!--list-separator-->

-  get-buffer-create:如果给定的buffer名不存在就创造一个buffer

    ```elisp

    ```

<!--list-separator-->

-  generate-new-buffer:总是创造一个新buffer并给出buffer名


#### check buffer exists {#check-buffer-exists}

<!--list-separator-->

-  get-buffer-name:如果buffer存在，返回buffer name的字符串；如果buffer不存在，返回nil
