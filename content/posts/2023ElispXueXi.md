+++
title = "Elisp学习"
author = ["lqs_is_a_goodboy"]
description = "elisp基础语法"
date = 2023-11-15T23:43:00+08:00
lastmod = 2024-02-20T20:56:42+08:00
tags = ["elisp学习", "emacs"]
categories = ["编程"]
draft = false
+++

tags
:


## Elisp学习 {#2023ElispXueXi}

tags
: 编程 emacs

source
: <http://smacs.github.io/elisp/>

date
: <span class="timestamp-wrapper"><span class="timestamp">&lt;2023-11-15 ���� 23:41&gt;</span></span>


### elisp的运行环境 {#elisp的运行环境}

-   只有在lisp-interaction-mode下，才能执行elisp代码
    -   M-x lisp-interaction-mode
    -   在行尾右括号后面按C-j
-   elisp不好作为可执行方式来来运行，所有的elisp都是在emacs的环境下


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


### elisp数据类型（[elisp#Lisp Data Types](https://www.gnu.org/software/emacs/manual/html_node/elisp/Lisp-Data-Types.html "Emacs Lisp: (info \"(elisp) Lisp Data Types\")")）(未完成) {#elisp数据类型-elisp-lisp-data-types-lisp-data-types----未完成}

-   **Lisp“object”是Lisp使用和操作的一段数据程序。 就我们的目的而言，“类型”或“数据类型”是一组可能的Objects。**
-   Emacs内置了一些基础的对象类型，其他的对象类型都是由基础的对象类型构造的，这些类型包括“integer”, “float”, “cons”, “symbol”, “string”, “vector”,“hash-table”, “subr”, “byte-code function”, and “record”
    -   每一种基础的类型都有一个与之相关的Lisp 函数来检查某个对象是否属于该类型
-   **Lisp 的对象的类型是隐式的由对象本身指明——————例如一个 Lisp vector 对象就是 vector 类型、一个 int 对象就是 int 类型， Lisp 知道Vector是Vector、int是int，不需要像其他语言一样声明对象（变量）类型**


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
-   \`\` **每个函数都有一个返回值。这个返回值一般是函数定义里的最后一个表达式的值。** ''


#### elisp变量：setq、defvar、let、let\* {#elisp变量-setq-defvar-let-let}

<!--list-separator-->

-  使用setq声明变量：elisp里的变量使用无需像C语言那样需要声明，你可以用setq直接对一个变量赋值

    -   **也就是说elisp变量没有类型（存疑）**

    <!--listend-->

    ```elisp
    (setq foo "I'm, foo")
    (message foo)


    (setq res (res * n) n (n - 1))  ;; *使用setq一次性为两个变量res 和 n赋值*
    ```

<!--list-separator-->

-  使用defvar声明变量（与setq有区别）

    ```elisp
    ;; (defvar 变量名 变量值
    ;;   变量文档)      //可省略 *如果添加了变量文档，使用C-h v查看该变量的时候会显示变量文档的内容*
    (setq foo "I'm a foo")             //此时foo的值为 I'm a foo
    (defvar foo "Did I have a value?"   //此时foo的值依然为 I'm a foo
      "A demo variable")
    foo                                //此时foo的值依然为 I'm a foo
    ```

    <!--list-separator-->

    -  **defvar 与 setq的区别**

        -   defvar可以为变量提供文档
        -   defvar声明的变量foo如果之前有一个值，那么使用defvar再次声明foo并赋值，foo的值仍然为之前的值

<!--list-separator-->

-  使用let/let\*声明局部变量

    ```elisp
    ;;let声明局部变量的语法
    (let (bindings)   ;; *bindings可以是(var value)这样对var赋初始值的形式，或者用var声明一个初始值为nil的变量*
      body)           ;; *一定要注意(let (temp 1))是错误的！！！！(let ((temp 1)))才是正确的，bindings是(key value)，(key value)外面的括号不可少，而(let (bindings))在bindings外面还有一层括号，所以是2层括号*


    (defun circle-area(radix)
      (let ((pi 3.1415926) area)           ;; *let此处声明了两个变量pi和area，其中变量pi使用(key value)形式声明并赋值，area未赋值*
        (setq area (* pi radix radix))
        (message "直径为%.2f的圆面积是%.2f" radix area)
        )
      )
    (circle-area 3)

    ;; let*和let的区别：let*可以在声明中使用前面声明的变量
    (defun circle-area1(radix)
      (let ((pi 3.1415926) (area (* pi radix radix)))  ;;此处使用(key value)形式为pi和area赋值，其中对area的声明使用了前面声明的pi变量
        )
    ```


#### lambda 表达式： **最常用的是作为参数传递给其它函数（lambda表达式本质是一个函数，但这个函数可以赋值给一个变量并作为变量被调用）** {#lambda-表达式-最常用的是作为参数传递给其它函数-lambda表达式本质是一个函数-但这个函数可以赋值给一个变量并作为变量被调用}

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


### 控制结构 {#控制结构}


#### 顺序执行 {#顺序执行}

```elisp
;; (progn A B C...)  ;;让ABC顺序执行
(progn
  (setq foo 3)
  (message "Square of %d is %d" foo (* foo foo)))
```


#### elisp 有两个最基本的条件判断表达式 if 和 cond {#elisp-有两个最基本的条件判断表达式-if-和-cond}

```elisp
(if condition
    then
  else)
;; *注意if后面执行的语句如果超过1句，一定要用progn，而不是直接括号括起来*
;; *注意是(if t)而不是(if (t))，后者是错误的，例如(if (numberp 1))是正确的、(if ((numberp 1)))是错误的*

(cond
 (case1 do-when-case1)
 (case2 do-when-case2)


 (t do-when-none-meet))
```


#### 循环使用的是 while 表达式。 {#循环使用的是-while-表达式}

```elisp
  ;;循环语法
  (while condition
    body)      ;; *目前的尝试表明，body可以超过1句且不需要括号括起来*


  ;;阶乘函数（不可用）
  (defun jiecheng(n)
    (let (temp 1)                     ;;错在这，let的语法不对，应该是(let (temp 1))
      (while (> n 1)
        (progn
          (setq temp (* temp n))
          (setq n (- n 1))
          ))
      ))

  ;;阶乘函数（可用）
  (defun jiecheng(n)
    (let ((temp 1))      ;;    (let bindings)--->(let ((key value)) ……   )
      (while (> n 1)     ;;    (while condition body) --> (while (> n 1) ……    )
        (setq temp (* temp n) n (- n 1))  ;;(setq var value) --> (setq temp (* temp n))
;;        (setq n (- n 1))     ;;;;(setq var value) --> (setq n (- n 1))
        )
      temp))
    (jiecheng 4)

```


### 逻辑运算：and、or、not(when、unless)（未完成） {#逻辑运算-and-or-not--when-unless--未完成}

-   **Lisp语言使用nil表示假，t表示真**
-   **在Elisp中，只有nil和空list会被认为是nil，其他的所有值都被认为是逻辑真**

<!--listend-->

```elisp
;;在elisp中可以像+一样计算or的值
(or nil 0 nil) ---> 0
(+ 1 0 1) ---> 2
;;普通的带参数的函数形式
(defun hello-world(name)
  (message "Hello,%s" name
  ))
(hello-world "Emacser")


;;使用or设置函数的缺省值
(defun hello-world (name)

  )
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

    (while  (&gt; n 1)
      (setq temp (\* temp n))
      (setq n (- n 1))
      )
    (while (&gt; n 1)
      (setq temp (\* temp n))
      )
      (setq n (- n 1))
