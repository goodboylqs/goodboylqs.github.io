+++
title = "Org-Roam-Bibtex的使用"
author = ["大黑"]
description = "使用Emacs+Zotero管理文献，方便做笔记与查询"
date = 2023-11-21T22:32:00+08:00
lastmod = 2023-12-24T13:18:47+08:00
tags = ["Emacs", "org-roam"]
categories = ["编程"]
draft = false
featured_image = "../picture/文献界面.png"
+++

tags
:


## Org-Roam-Bibtex的使用 {#2023OrgroambibtexDeShiYong}

-   配置方法：<https://www.bilibili.com/video/BV1Mg4y1j75u/?spm_id_from=333.337.search-card.all.click&vd_source=90a44733b775f9f2aeec92b479575071>
    -   使用Zotero实现对文献的综合管理，包括文献的命名、文献附件的存储、文献导出格式的规定等
        -   使用Betterbibtex来对导出的bib文件的格式进行规定，包括规定bib文件中没一个条目包含的内容
            ```nil
              @book{2023JiChuHuiJiDi6Ban,
              title = {基础会计（第6版）},
              author = {陈国辉，迟旭升},
              year = {2023},
              month = nov,
              publisher = {{东北财经大学出版社}},
              file = {/home/goodboylqs/Nutstore Files/Zotero-Library/2023JiChuHuiJiDi6Ban.pdf}
            }
            ```

            -   注意事项
                -   编辑引用格式为year+shorttitle(3,3)，这样每个条目的citekey就会被设置为year+shorttitle(3,3)，例如上述的2023JiChuHuiJiDi6Ban

                    {{< figure src="/ox-hugo/2023-11-21_22-06-26_1700575559915.png" >}}
                -   **在添加条目的时候，一定要注意标题和日期是必填的**
        -   使用Zotfile来规定条目对应的pdf的命名格式和存储路径，实现pdf文件的云端备份
            -   renaming rules设置为%b，含义是将pdf的命名格式设置成与Betterbibtex导出的bib文件的citekey一样，例如上述的file = {/home/goodboylqs/Nutstore Files/Zotero-Library/2023JiChuHuiJiDi6Ban.pdf}中的2023JiChuHuiJiDi6Ban.pdf
            -   custom locatios设置pdf的存储路径，这个要与坚果云的同步文件夹路径协同，利用好坚果云的备份实现pdf文件的云端备份
-   使用方法
    1.  先打开Zotero，新建一个条目，编辑条目的标题、摘要、链接等内容。注意标题和日期是必填的，这关系到后面的文件引用格式
    2.  右键点击Zotero中新建的条目，添加添加附件--文件副本，然后对应的文献附件就会按照配置好的命名格式和存储路径进行存储
    3.  右键包含所有文献的文件夹，点导出，选择保持更新
    4.  打开Emacs，M-x helm-bibtex，就能看到已经有的文献。 **但是在该条目没有笔记的时候，不能选中该条目然后tab然后f8编辑笔记。在该条目没有笔记的时候，要C-c n k(orb-insert-link)来为该条目添加笔记，这样才会调出你配置好的zotero文献模板。**
    5.  当你为条目添加了笔记后，可以①使用helm-bibtex②C-x n f(org-roam-node-find)来查看已经建立的笔记
    6.  打开一条笔记后，在对应的org headline下面，使用org-noter，既可以调出文献对应的pdf附件并做笔记
