---
title: "OS X下卸载R语言"
date: 2015-05-16 23:05:18
tags: Mac
categories: 技术
---
我最讨厌那些装上后不能完全卸载的软件，比如R。如果你也需要完全卸载R，下面的方法也许会帮到你。  

OS X 下R语言安装后会有三部分内容（默认）:

* R framework（/Library/Frameworks/R.framework）
* R.app（/Applications/R.app，可选）
* Tcl/Tk（/usr/bin，可选）

总的来说，前两个组件是比较容易删除的（需要权限的话，加sudo）:
`rm -rf /Library/Frameworks/R.framework /Applications/R.app 
   /usr/bin/R /usr/bin/Rscript`
最恶心的就是第三个组件，官网
[Uninstalling under OS X](http://cran.r-project.org/doc/manuals/R-admin.html#Uninstalling-under-OS-X)竟然说卸载它不容易，然后只给出了查看它安装了哪些文件，然后就没然后了。

我的方法，也只是在它的基础上实现的，至于会不会产生不良后果现在还不知道，如果不在意这些细节的话，建议不要删除算了。

1. 查看安装了哪些文件并将结果重定向到文件。  
`pkgutil --files org.r-project.x86_64.tcltk.x11 > tcltk`  
2. 查看一下文件内容，最好用文本编辑器打开，因为我们还要修改下这个结果。 

        usr
        usr/local
        usr/local/bin
        usr/local/bin/tclsh8.6
        usr/local/bin/wish8.6
        usr/local/include
        usr/local/include/fakemysql.h
        usr/local/include/fakepq.h
        usr/local/include/fakesql.h
        usr/local/include/itcl.h  
  这是文件的前10行。有两点需要注意：  
      * 都是相对路径
      * 有目录、有文件  
  首先我们要剔除掉里面的一些目录（放心，没几个），这里为了保险起见我手工删除的，比如`usr`、`usr/local`、`usr/local/bin`、`usr/local/include`这些都是要排除掉的目录，因为Tcl/Tk影响的都是它们的内部的子目录或文件。  
3. 最重要的是第2步，一定要细心排除掉那些我们不想删掉的目录。这一步是把相对路径变成绝对路径，采用Vim或Sublime等，在每一行的行首加上`/`。
4. `cat tcltk | sudo xargs rm -rf`。

***忠告：`rm -rf`是一个非常危险的命令。***
 
 
