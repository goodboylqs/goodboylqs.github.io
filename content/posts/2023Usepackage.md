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


### after关键字：指定包c在包a加载之后再加载 {#after关键字-指定包c在包a加载之后再加载}

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
