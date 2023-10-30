+++
title = "Emacs的Use-Package用法"
author = ["553912917@qq.com"]
tags = ["emacs", "use-package"]
categories = ["emacs", "编程"]
draft = false
+++

tags
: autoload,deferred-loading,emacs,emacs-lisp,handler,keymap,keyword,melpa,ues-package


## Use-Package {#2023Usepackage}


### 关于指定包的加载顺序 {#关于指定包的加载顺序}


#### after关键字：指定包c在包a加载之后再加载 {#after关键字-指定包c在包a加载之后再加载}

> (use-package ivy-hydra
> :after (ivy hydra))
> In this case, because all of these packages are demand-loaded in the order they occur, the
> use of :after is not strictly necessary.

为了让ivy-hydra在ivy和hydra加载之后再加载，就需要用到:after关键字。注意对于ivy和hydra的加载语句还是必要的，只是使用了after关键字之后，就不需要必须把加载ivy和hydra的语句放在加载ivy-hydra的语句之前了


### require关键字：如果包a的依赖项不满足则阻止包a的加载 {#require关键字-如果包a的依赖项不满足则阻止包a的加载}

\`\`Prevent loading if dependencies are missing''
当包a需要的加载需要依赖包c时，如org-roam-bibtex的加载需要依赖Org-roam和ivy-bibtex，则需要通过require关键字来指定


### pin关键字：指定你的package来自哪个池子（melpa、gnu、其他） {#pin关键字-指定你的package来自哪个池子-melpa-gnu-其他}

> (use-package company
> :ensure t
> :pin gnu)
> (use-package evil
> :ensure t)
> ;; no :pin needed, as package.el will choose the version in melpa
> (use-package adaptive-wrap
> :ensure t
> ;; as this package is available only in the gnu archive, this is
> ;; technically not needed, but it helps to highlight where it
> ;; comes from
> :pin gnu)

-   默认情况下，不使用pin特定指明，默认所有的包来自melpa
-   可以使用pin来指定包来自gnu


### ensure-system-package关键字：允许你安装package的时候自动安装package依赖的系统软件 {#ensure-system-package关键字-允许你安装package的时候自动安装package依赖的系统软件}

> The :ensure-system-package keyword allows you to ensure system binaries exist alongside
> your package declarations.

-   从use-package的version 2.0开始，use-package的一些关键字可以视为use-package这个框架的extension， **想使用这些关键字要先安装对应的extension**
    -   想使用关键字use-package-ensure-system-package，先安装对应的extension————
        ```elisp
        (use-package use-package-ensure-system-package
        :ensure t)
        ```
    -   安装package rg的时候想要安装系统软件rg
        ```elisp
        (use-package rg
          :ensure-system-package rg)   ;;相当于执行了apt install rg
        ```
        当要安装的软件的包名和二进制软件名不一致的时候
        ```elisp
        (use-package rg
          :ensure-system-package (binary . package-name))
        ```
-   **可以自定义要执行的软件安装命令**

    -   如果自定义了要执行的命令就执行自定义的命令，如果只写了要安装的包，并没有指定安装命令，那么use-package会执行默认的安装命令

    <!--listend-->

    ```elisp
    (use-package ruby-mode
      :ensure-system-package
      ((rubycop . "gem install rubocop"))    ;;自定义执行的安装命令是gem install rubycop
      )
    ```


### 关于包加载前/后在执行的命令 {#关于包加载前-后在执行的命令}


#### init关键字：在加载包之前执行的命令 {#init关键字-在加载包之前执行的命令}

\`\`Use the :init keyword to execute code before a package is loaded.''

```elisp
(use-package foo
  :init
  (setq foo-variable t))
```


#### config关键字：在加载包之后执行的命令 {#config关键字-在加载包之后执行的命令}

\`\`Similarly, :config can be used to execute code after a package is loaded.''

```elisp
(use-package foo
  :init
  (setq foo-variable t)
  :config
  (foo-mode 1))
```


### 关于快捷键的绑定 {#关于快捷键的绑定}


#### bind关键字：全局绑定和mode-map绑定 {#bind关键字-全局绑定和mode-map绑定}

> :bind (("M-s O" . moccur)
> :map isearch-mode-map
> ("M-o" . isearch-moccur)
> ("M-O" . isearch-moccur-all))

```elisp
:bind (("M-s O" . moccur)  ;;绑定全局的快捷键
       :map isearch-mode-map
       ("M-o" . isearch-moccur)  ;;绑定只在isearch-mode下适用的快捷键
       ("M-O" . isearch-moccur-all))
```


### mode和interpreter关键字：打开某类型文件后执行的命令 {#mode和interpreter关键字-打开某类型文件后执行的命令}

> Similar to :bind , you can use :mode and :interpreter to establish a deferred binding
> within the auto-mode-alist and interpreter-mode-alist variables.

```elisp
  ;; The package is "python" but the mode is "python-mode":
(use-package python
  :mode ("\\.py\\'" . python-mode)
  :interpreter ("python" . python-mode))
```


### hook关键字：为package的hooks添加上要执行的命令 {#hook关键字-为package的hooks添加上要执行的命令}

\`\`The :hook keyword allows adding functions onto package hooks.''

-   package的hook，例如prog-mode-hook，即编程的hook。以下两种写法的效用是等价的
    ```elisp
    ;;写法一：用hook
    (use-package company
         :hook ((prog-mode text-mode) . company-mode)) ;;写法二：用command和init
    (use-package company
         :commands company-mode   ;;再运行一下company-mode
         :init
         (add-hook 'prog-mode-hook #'company-mode)  ;;用init在加载company-mode前线将company-mode与到prog-mode联系起来，确保prog-mode打开的时候company-mode也打开，
         (add-hook 'text-mode-hook #'company-mode)
         )
    ```


### custom关键字：自定义package所带的变量的值 {#custom关键字-自定义package所带的变量的值}

\`\`The :custom keyword allows customization of package custom variables.''
