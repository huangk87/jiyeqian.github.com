---
layout: post
title: "Aquamacs速查手册"
description: ""
category: 振导社会
published: true
tags: [emacs, 简介, 工具, 速查, 程序, lisp]
---
{% include JB/setup %}

[Aquamacs](http://Aquamacs.org)是[GNU Emacs](http://www.gnu.org/software/emacs/)在OSX系统的一个发行版，除了支持Emacs标准快捷键外，也支持与OSX系统兼容的快捷键。因此，本文的大多数内容对Emacs也实用。

>While the useful Emacs key bindings are supported and Emacs packages usually just work, Aquamacs is adapted to be comparable to younger text editors (than Emacs). It also comes with a range of packages that give you greater comfort in editing certain types of files.

##基础知识

###键盘对应关系

Emacs | Mac | PC
----|----|----
META | esc/option | Alt
CTRL | command | Ctrl

###获取帮助的快捷键

快捷键 | 功能
---- | ----  
`C-h t` | emacs tutorial 
`C-h i` | emacs info 
`C-h r` | emacs manual  
`C-h f <函数名>` | 函数的帮助信息  
`C-h v <变量名>` | 变量的帮助信息  
`C-h k <快捷键>` | 快捷键的帮助信息

Emacs最常用功能可通过简明的[GNU Emacs Reference Card](http://www.damtp.cam.ac.uk/user/eglen/ess11/resources/emacs-refcard.pdf)学习。

###info的快捷键

功能 | 快捷键 | 快捷键 | 功能
----: | ----: | :---- | :----
上一节点 | `p` | `n` | 下一节点   
上一菜单 | `[` | `]` | 下一菜单（菜单前标有*）
交叉引用跳转 | `f` | `l` | 交叉引用返回
向上翻页 | `DELETE` | `SPACE` | 向下翻页   
||`C-l` | 当前节点循环翻页
||`b` | 回到节首   
||`u` | 上级菜单
||`m` | 菜单 
||`g` | 跳到指定节点
||`t` | 回到顶层菜单
||`d` | 回到info总目录
||`h` | info tutorial 

info兼容Emacs快捷键。

<!--
###Emacs戏法

 快捷键 | 功能
---- | ----
`C-u <数字> 快捷键`| 为快捷键传送参数
`M-<数字> 快捷键`| 执行快捷键`<数字>`次

如何输入`<数字>`个数字？

拼写检查：   
ispell、flyspell-mode

Tabs and Windows：
一个tab对应于一个buffer，OSX切换tab的快捷键是command+数字
Aquamacs配置文件的位置

可用C-h v load-path查看  
-->

###OSX系统和Emacs术语对应关系 

OSX Term | Emacs Term     
:--------|----------  
Window    | Frame      
Tab/pane  | Window    
Document  | Buffer    
Cursor    | Point    
Mouse pointer     | Pointer  
Keyboard shortcut | Key (binding)  

###Aquamacs相关文件 
插件目录： 
{% highlight bash %}
~/Library/Application Support/Aquamacs Emacs/myPlugin
{% endhighlight %}

配置文件目录：
{% highlight bash %}
/Library/Application Support/Aquamacs Emacs/  
/Library/Preferences/Aquamacs Emacs/  
~/Library/Preferences/Aquamacs Emacs/  
{% endhighlight %}

用户自定义配置文件：
{% highlight bash %}
~/.emacs
~/Library/Preferences/Aquamacs Emacs/Preferences.el
{% endhighlight %}

当然，用户可以在配置文件中指定目录和相关文件位置。


##Aquamacs常用配置

Aquamacs已经完成了很多Emacs的基本配置，配置中需要的相关elisp函数，[可查索引](http://www.gnu.org/software/emacs/manual/html_node/elisp/Index.html)。[ahei][]和[alexott][]的配置是不错的参考，[emacswiki][]和[emacser][]可以找到很多相关资料。

[el-get][]是一个与debian的`apt-get`类似的emacs配置包管理器。

[ahei]:http://code.google.com/p/dea/
[alexott]:https://github.com/alexott/emacs-configs
[emacser]:http://emacser.com/
[emacswiki]:http://www.emacswiki.org/   
[el-get]:https://github.com/dimitri/el-get


<!--
{% highlight cl %}
(set-default-font "Menlo Plus")
(set-fontset-font
 (frame-parameter nil 'font)
 'han
 (font-spec :family "Hiragino Sans GB"))
{% endhighlight %}

{% highlight cl %}
(set-fontset-font NAME TARGET FONT-SPEC &optional FRAME ADD)
{% endhighlight %}


- `NAME`
- `TARGET`
- `FONT-SPEC`可以通过函数`font-spec`设置，参数和`set-face-attribute`的一致
- `FRME ADD`

查看当前用的字体可以用`describe-font`
-->


###基本配置

加载配置文件的路径：

{% highlight cl %}
(add-to-list 'load-path "~/.emacs.d/site-lisp")
{% endhighlight %}

绑定快捷键到函数/命令：

{% highlight cl %}
(global-set-key (kbd "C-c c") 'comment-region-or-line) 
{% endhighlight %}


###代码补齐

代码补齐就是根据当前的输入字符，补全或提示即将要输入的字符。补齐的原理就是根据当前已输入的字符，在特定的字典里匹配出即将要输入的字符，因此，生成用于补齐的字典是关键。这个字典来源有两方面：一是，用户提前提前定义，[auto-complete][]的用户定义字典在dict目录，[yasnippet][]的代码片断预先定已在snippets目录，[emacs-template][]也有用于存放用户定义模板的templates目录；二是，程序自动分析生成，程序自动分析C/C++代码生成字典的有[cedet]和[gccsense][]，[rope][]可分析python代码生成补齐字典，还有一些自动生成补齐字典的方案与程序语言无关，通过当前缓冲区、文件等信息，自动生成补齐字典，比如[auto-complete][]和[company][]。

[auto-complete][]和[company][]主要功能是作为一个补齐前端，用户可以根据需要自定义用于补齐的后端驱动引擎，比如把[yasnippet][]（[解决company与yasnippet整合冲突的方案](http://www.emacswiki.org/cgi-bin/emacs-en/CompanyMode#toc8)）、[ropemacs][]和[cedet][]整合到它们的补齐方案中。[cedet][]（Collection of Emacs Development Environment Tools）结合[ecb][]（Emacs Code Browser）就是是一个功能完善的IDE方案，[cedet][]既有不错的前端，后端有有强大的C/C++代码解析引擎，[ecb][]方便阅读代码。

[yasnippet]:https://github.com/capitaomorte/yasnippet   
[emacs-template]:http://emacs-template.sourceforge.net   
[gccsense]:http://cx4a.org/software/gccsense/  
[ecb]:http://ecb.sourceforge.net/  

<!--
my-cedet-complete和company可以共存   
all-auto-complete-settings和company不可共存   
all-auto-complete-settings和my-cedet-complete不可共存   

Symbol's function definition is void: company-pysmell  
-->

[auto-complete]:http://cx4a.org/software/auto-complete/     
[company]: http://nschum.de/src/emacs/company-mode/   
[cedet]:http://cedet.sourceforge.net/  

###python的补齐方案
python的补齐方案基于[rope][]，python补齐首先需要安装[rope][]、[ropemode][]和[pymacs][]（安装时需要手动将Pymacs.py拷贝到python的site-packages目录）。在此基础上，可以安装[pysmell][]、[ropemacs][]或[pycomplete][]三者之一实现补齐。[company][]和[auto-complete][]这些补齐前端也可用[pysmell][]、[ropemacs][]作为其补齐的引擎。


[rope]:http://rope.sourceforge.net/   
[pycomplete]:http://www.rwdev.eu/articles/emacspyeng   
[pymacs]:https://github.com/pinard/Pymacs/   
[ropemode]:http://pypi.python.org/pypi/ropemode   
[pysmell]:http://code.google.com/p/pysmell/   
[ropemacs]:http://rope.sourceforge.net/ropemacs.html


##参考资源

[Aquamacs Emacs Manual](http://aquamacs.org/features.shtml)     
[emacswiki](http://www.emacswiki.org/)       
[GNU Emacs manual](http://www.gnu.org/software/emacs/manual/emacs.html)   
[叶文彬：Elisp入门](http://www.newsmth.net/bbsanc.php?path=%2Fgroups%2Fcomp.faq%2FEmacs%2Felisp%2Fhappierbee%2FM.1184679743.j0&ap=64311)   
[About Cons Cells](http://cs.gmu.edu/~sean/lisp/cons/)    
[wikipedia: S-expression](http://en.wikipedia.org/wiki/S-expression)      
[wikipedia: LISP](http://zh.wikipedia.org/wiki/LISP)  
[wikipedia: Lisp (programming language)](http://en.wikipedia.org/wiki/Lisp_(programming_language))   
[An Introduction to Emacs Lisp Programming](http://www.gnu.org/software/emacs/emacs-lisp-intro/)   
[GNU Emacs Lisp Reference Manual](http://www.gnu.org/software/emacs/manual/elisp.html)                
[Emacs补全利器：auto-complete+gccsense](http://emacser.com/emacs-gccsense.htm)   
[用CEDET浏览和编辑C++代码](http://emacser.com/cedet.htm)    
[手把手教你emacs cedet C/C++自动补全](http://www.cnblogs.com/logicbaby/archive/2011/10/19/2217253.html)   
[A Gentle introduction to CEDET](http://alexott.net/en/writings/emacs-devenv/EmacsCedet.html)   
[Emacs补全利器：auto-complete+gccsense](http://emacser.com/emacs-gccsense.htm)  

 
