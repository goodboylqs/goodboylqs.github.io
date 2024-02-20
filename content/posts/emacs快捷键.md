+++
title = "emacs快捷键"
author = ["lqs_is_a_goodboy"]
description = "emacs的常用快捷键"
date = 2023-12-21T08:00:00+08:00
lastmod = 2024-02-20T16:21:24+08:00
tags = ["emacs", "git"]
categories = ["编程"]
draft = false
featured_image = "../picture/git工作区.jpg"
+++

-   **使用ivy的时候，由于自动补全而导致无法创建包含已有关键字的新条目，使用C-M-j**


## org-noter {#org-noter}

-   M-e(org-noter-insert-precise-note) : 在选中区域做精确笔记
-   e/i(org-noter-insert-note)：可以为已经有的笔记项添加笔记


## 属性快捷键 {#属性快捷键}

-   创建多个eshell buffer
    -   C-u M-x eshell
-   在emacs外部打开
    -   C-c e (centaur-tabs-open-in-external-application)
-   **C-M-\\ (indent-region)：对齐代码**
    -   M-h (org-mark-element)：选中代码块
    -   可以先M-h选中要对齐的代码然后C-M-\\
-   **M-S-Left/Right：当前标题及其子标题的升降级**
-   C-c C-x p (org-set-property) ?

    Set a property. This prompts for a property name and a value. If necessary, the property drawer is created as well.
-   C-u M-x org-insert-drawer
-   C-c C-c (org-property-action)
-   C-c C-c s (org-set-property) ?

    Set a property in the current entry. Both the property and the value can be inserted using completion.


### 手册 {#手册}

-   ****大部分属性是非继承的，因为如果默认为继承的话，在属性搜索的时候会因为搜索到的内容太多超载**** ，你也可以定义变量org-use-property-inheritance的值来实现继承
-   属性是键值对。当它们与单个条目或一棵树相关联时，需要将它们插入名为“PROPERTIES”的特殊抽屉（参见抽屉）中，该抽屉必须位于标题及其规划线的正下方（参见截止日期和调度）适用时。每个属性都在一行中指定，首先是键（用冒号括起来），然后是值。键不区分大小写。这是一个例子：

<!--listend-->

```nil
* CD collection
** Classic
*** Goldberg Variations
  :PROPERTIES:
  :Title:     Goldberg Variations
  :Composer:  J.S. Bach
  :Artist:    Glenn Gould
  :Publisher: Deutsche Grammophon
  :NDisks:    1
  :END:
```

-   你可以设置属性'A'的值域'A_ALL'————定义允许的值后，设置相应的属性会变得更容易，并且不容易出现输入错误。对于CD收藏的示例，我们可以像这样预先定义出版商Publisher和盒子中的磁盘数量———— ****这样设置的属性默认是继承的****
    ```nil
    ​* CD collection
    :PROPERTIES:
    :NDisks_ALL:  1 2 3 4
    :Publisher_ALL: "Deutsche Grammophon" Philips EMI
    :END:
    ```
-   可以为整个文件设置属性，这需要你把属性放在文件开头，类似下面
    ```nil
    #+PROPERTY: NDisks_ALL 1 2 3 4
    ```
-   如果你想为已经存在的属性A添加一个值，在属性名后面使用'+'，下面例子表示属性var的值为'foo=1 bar=2'
    ```nil
    #+PROPERTY: var  foo=1
    #+PROPERTY: var+ bar=2
    ```
-   使用全局变量 org-global-properties 设置的属性值可以被所有 Org 文件中的所有条目继承。


## column view {#column-view}


#### 设置column view {#设置column-view}

-   设置列视图首先需要定义列。这是通过定义列格式行来完成的。
    -   要指定仅适用于特定树的格式，请将“COLUMNS”属性添加到该树的顶部节点，例如：
        ```nil
        ** Top node for columns view
        :PROPERTIES:
        :COLUMNS: %25ITEM %TAGS %PRIORITY %TODO
        :END:
        ```
    -   第一个标题之前的属性抽屉中的“COLUMNS”属性将应用于整个文件。作为属性抽屉的补充，还可以使用如下行为整个文件定义关键字
        ```nil
        #+COLUMNS: %25ITEM %TAGS %PRIORITY %TODO
        ```
    -   如果条目中存在“COLUMNS”属性，它会为条目本身及其下方的整个子树定义列。由于列定义是文档层次结构的一部分，因此当您编辑树的较深部分时，您可以在级别 1 上定义对所有子级别足够通用的列，并在更下方定义更具体的列。
-   定义列的属性
    -   除了%和属性名都是可选的
        ```nil
        %[WIDTH]PROPERTY[(TITLE)][{SUMMARY-TYPE}]
        ```

        -   SUMMARY-TYPE的可选内容
            ‘+’	        Sum numbers in this column.
            ‘+;%.1f’	Like ‘+’, but format result with ‘%.1f’.
            ‘$’	Currency, short for ‘+;%.2f’.
            ‘min’	Smallest number in column.
            ‘max’	Largest number.
            ‘mean’	Arithmetic mean of numbers.
            ‘X’	Checkbox status, ‘[X]’ if all children are ‘[X]’.
            ‘X/’	Checkbox status, ‘[n/m]’.
            ‘X%’	Checkbox status, ‘[n%]’.
            ‘:’	Sum times, HH:MM, plain numbers are minutes.
            ‘:min’	Smallest time value in column.
            ‘:max’	Largest time value.
            ‘:mean’	Arithmetic mean of time values.
            ‘@min’	Minimum age56 (in days/hours/mins/seconds).
            ‘@max’	Maximum age (in days/hours/mins/seconds).
            ‘@mean’	Arithmetic mean of ages (in days/hours/mins/seconds).
            ‘est+’	Add low-high estimates.
-   一个例子
    ```nil
    :COLUMNS:  %25ITEM %9Approved(Approved?){X} %Owner %11Status \
                       %10Time_Estimate{:} %CLOCKSUM %CLOCKSUM_T
    :Owner_ALL:    Tammy Mark Karl Lisa Don
    :Status_ALL:   "In progress" "Not started yet" "Finished" ""
    :Approved_ALL: "[ ]" "[X]"
    ```


#### 开启column view {#开启column-view}

-   C-c C-x C-c 开启
-   C-c C-c 关闭
-   n or S-RIGHT (org-columns-next-allowed-value)
    p or S-LEFT (org-columns-previous-allowed-value)

        Switch to the next/previous allowed value of the field. For this, you have to have specified allowed values for a property.
    e (org-columns-edit-value)

        Edit the property at point. For the special properties, this invokes the same interface that you normally use to change that property. For example, the tag completion or fast selection interface pops up when editing a ‘TAGS’ property.
    C-c C-c (org-columns-toggle-or-columns-quit)

        When there is a checkbox at point, toggle it. Else exit column view.
    v (org-columns-show-value)

        View the full value of this property. This is useful if the width of the column is smaller than that of the value.
    a (org-columns-edit-allowed)

    Edit the list of allowed values for this property. If the list is found in the hierarchy, the modified values is stored there. If no list is found, the new value is stored in the first entry that is part of the current column view.

    &lt; (org-columns-narrow) ?
    &gt; (org-columns-widen)

        Make the column narrower/wider by one character.
    S-M-RIGHT (org-columns-new) ?

        Insert a new column, to the left of the current column.
    S-M-LEFT (org-columns-delete) ?

    Delete the current column.


## 链接 {#链接}

-   内部链接
    -   先在要链接的地方用<span class="org-target" id="org-target--A"></span>锚定
    -   链接
        ```nil
        [[A]]
        ```
-   外部链接
    -   链接到headline
        先用C-c l存储要链接到的headline的地址，然后C-c C-l在要插入链接的地方插入链接，粘贴已经存储好的headline的地址


## 表格 {#表格}

| 按键                 | 功能                                    |
|--------------------|---------------------------------------|
| C-c C-c（怎么这个键哪里都有？！） | 重新对齐表格                            |
| TAB/S-TAB            | 重新对齐表格，并在横向格子里跳转Ret重新对齐表格，并跳到下一行的格子 |
| M-方向键             | 将单元格所在的行/列进行上下移动         |
| M-S-←/↑              | 删除光标所在的行列（记忆：这两个箭头都是指向表格内部，用于缩减表格，即删除） |
| M-S-→/↓              | 在光标所在的单元格前面或者上面，增加行或列 |
| C-c -或者C-Ret       | 新增一个分隔线，两个按键在生成分隔线后的光标行为不同C-c ^对表格进行行排序 |

-   将内容转化为表格： **org-table-create-or-conver-from-region(C-c |)**,直接选中要转化为表格的区域，然后执行这个命令即可


## 代码块 {#代码块}

-   21. C-w 剪切


### 在org-mode中插入src代码块 {#在org-mode中插入src代码块}

-   C-c C-,(org-insert-structure-template)


### 在org-mode中的src区域运行代码 {#在org-mode中的src区域运行代码}

-   将光标移动到src区域，按C-c C-c即可运行代码
-   将光标移动到src区域，按C-c '可对代码进行编辑


## 窗口 {#窗口}

-   20. ****调整窗口大小****
    -   \`C-x ^’ makes the current window taller (‘enlarge-window’)
    -   \`C-x }’ makes it wider (‘enlarge-window-horizontally’)
    -   \`C-x {’ makes it narrower (‘shrink-window-horizontally’)


## 时间 {#时间}

-   C-u C-c . ：插入带时分的时间戳


## 其他 {#其他}

-   19. 倒计时：org-timer-set-timer，计时结束会弹出提示
-   18. ****undo:C-/ redo:C-?****
-   17. 在org roam buffer中
    -   alt-2是收起所有项
    -   alt-3是展开所有项
-   16.C-up/down 快速到最上面/下面
-   15. C-x n f org-roam-node-find
    -   C-x n i org-roam-node-insert
-   14. 翻译：C-c t
-   13. 选项卡操作
    -   ("C-f" . centaur-tabs-backward)
    -   ("C-b" . centaur-tabs-forward)
    -   ("C-c f" . centaur-tabs-backward-group)
    -   ("C-c b" . centaur-tabs-forward-group)
    -   ("C-c w" . kill-this-buffer)

-12. ****C-c C-w将一个headline的内容移动移动到另一个headline下****
-11. C-c C-o打开光标处链接，C-c C-l插入链接或修改光标处链接
-10. C-u 数字 C-x Tab Left/Righ 对选中内容进行缩进数字个数
-9. C-x r b 跳转书签
-8. C-x r m 设置书签
-7. M-! 直接执行shell脚本命令
-6. C-u C-c C-c标签对齐
-5. 插入时间戳：C-c .
-4. 插入tag： C-c C-q
-3. C-c p l：在指定目录对应的项目下进行搜索
-2. 选中区域退格：M-Left/Right
-1. C-M-\\ 选中区域自动对齐

1.  M-; 为选中区域添加注释
2.  C-x p 调用projectile-find-file
    -   C-c p s r调用projectile-rgrep
3.  C-x ^ 调节窗口高度
4.  C-backspace可以删除前面的一句话,C-delete可以删除后面的一句话
5.  C-' avy快速跳转
6.  M-a     句首
    -   M-e     句尾
7.  C-S-Backspace  删除当前行
8.  M-d     删除光标后的单词
    -   M-DEL   删除光标前的单词
9.  M-h     标记整个段落
    -   C-x C-p 标记整个页面
    -   C-x h   标记整个缓冲区
10. C-x t 翻译选中单词
11. C-c t 开启/关闭one window
12. C-c right/left 切换窗口
13. C-x C-x选中光标所在位置与上一个mark point之间的区域
