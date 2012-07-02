---
layout: post
title: "基于jekyll的github建站指南"
description: ""
category: 振导社会
published: true
tags: [github, git, jekyll, 速查, 简介, 程序]
---
{% include JB/setup %}

简介：本文主要介绍利用jekyll在github上搭建站点的方法与原理。


##jekyll驱动网站原理

github提供了[自动](https://help.github.com/articles/creating-pages-with-the-automatic-generator)和[手动](https://help.github.com/articles/creating-project-pages-manually)建站的文档。github的网站是位于username.github.com仓库中的静态页面。因此，利用高效的工具生成高质量的静态页面是github建站的王道。[jekyll](http://jekyllrb.com/)就是用[ruby](http://www.ruby-lang.org/)制造的这样的利器（构建网站时ruby不是必会的）。[用Jekyll构建静态网站的方法](http://chen.yanping.me/cn/blog/2011/12/15/building-static-sites-with-jekyll/)可作参考。

<!--more-->

模板、数据和美化是jekyll建站的三要素。jekyll自动将数据注入模板，通过美化，得到站点。这里可以看到[一堆用jekyll打造的站点](https://github.com/mojombo/jekyll/wiki/Sites)，大部分都可以`git clone`回来学习参考。

###jekyll的模板
jekyll模板就是带有变量的html格式文件。jekyll的模板用[Liquid](http://liquidmarkup.org/)语言描述，定义了页面框架。Liquid通过（Output）和标签（Tag）两种标记（markup），即可描述页面的结构，具体可参阅[Liquid for Designers](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers)。jekyll对Liquid的标签进行了[扩展](https://github.com/mojombo/jekyll/wiki/Liquid-Extensions)。Liquid模板语言对模板数据描述（当然也需要html），生成了站点页面的骨架。Liquid所需要的jekyll的[模板数据][TD]在这儿可以找到。除此之外，[YAML Front Matter][YFM]定义的变量也可以作为模板数据。

[TD]: https://github.com/mojombo/jekyll/wiki/template-data   
[YFM]: https://github.com/mojombo/jekyll/wiki/YAML-Front-Matter

###jekyll的数据
jekyll的数据是用[markdown](http://daringfireball.net/projects/markdown/)、[textile](http://textile.sitemonks.com/)等标记语言写的文档。这些格式的文档解析为html格式后，注入jekyll模板的<code>&#123;&#123; content &#125;&#125;</code>，就有了网站的页面。jekyll支持的markdown解析器（将markdown转换为html）有：<span id="markdown"></span>[rdiscount](https://github.com/rtomayko/rdiscount/)、[kramdown](http://kramdown.rubyforge.org/)、[redcarpet](https://github.com/tanoku/redcarpet/)、[maruku](http://maruku.rubyforge.org/)（jekyll默认）、[bluecloth](http://deveiate.org/projects/BlueCloth/)。为了方便处理公式，也有人hack了jekyll，[将pandoc作为markdown的解析器](http://yangzetian.github.com/2012/04/15/jekyll-pandoc/)。一个更方便的方法是通过[jekyll-pandoc-plugin](https://github.com/dsanson/jekyll-pandoc-plugin)插件，启用pandoc解析器。但是，这些hack后的jekyll启用pandoc不会在github上生效，只能用于本机。

###jekyll的美化
美化就是用CSS和javascript对html描述的页面进行渲染，美化网页。[twitter bootstrap](http://twitter.github.com/bootstrap/)是一套极易上手的页面美化工具。twitter bootstrap还提供了[960网格布局](http://960.gs/)，只要按照它约定的方式对页面结构定义、对html标签的`class`命名，网页的布局美化可谓快又好。

##在jekyll-bootstrap基础上搭建网站

jekyll只是定义了一套github生成网站的规程，并且提供了相应的工具。如果按照这套法则，从头开始仍然麻烦。[jekyll-bootstrap](http://jekyllbootstrap.com/)就是一个jekyll构建站点的demo，在此基础上学习修改，相当快捷容易。除此之外，[octopress](http://octopress.org/)也是基于jekyll上的不错的工具套件。

###目录结构
jekyll-bootstrap的[目录结构](https://github.com/mojombo/jekyll/wiki/usage)中，只需要将markdown文档放到\_posts目录下，`jekyll --server`，即可在http://localhost:4000看到页面内容，见[快速入门示范](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)。\_posts中的数据文档，通过注入\_layouts定义的模板（\_layouts的模板一般指向了\_includes/themes中的模板），渲染页面的CSS和JS文档在assets/themes中，一些解析markdown用到的ruby插件放在\_plugins目录。通过`jekyll --server`最终生成的静态页面在\_sites目录，但是，在更新站点的时候`git push`推送的是其上层目录，而不需要_sites目录，页面的解析留给github做了。

jekyll-bootstrap的\_includes/JB中有一些常用的工具，用于列表显示、评论等，可参看\_includes/themes中主题的相关html文档学习。

\_includes/themes中的主题一般包含default.html、post.html和page.html三个文档。default.html定义了网站的最上层框架（模板），post.html和page.html是其子框架（模板）。`rake page`命令生成的页面按page.html解析，markdown文件的[YAML Front Matter][YFM]中`layout: page`；`rake post`命令生成的页面按post.html解析，markdown文件的[YAML Front Matter][YFM]中`layout: post`。生成好的html子页面通过default.html的<code>&#123;&#123; content &#125;&#125;</code>变量调用，生成整个页面。

###自定义
站点生成需要用到\_config.yml[配置文件](https://github.com/mojombo/jekyll/wiki/configuration)，`markdown`变量定义了解析markdown用的[解析器](#markdown)。当然也可自定义变量，比如在`JB:`下定义一个二级变量`RSS_path: /atom.xml`（注意缩进）。在html页面中，就可以用<code>&#123;&#123; site.JB.RSS_path &#125;&#125;</code>得其值。[YAML Front Matter][YFM]中也可以自定义变量，其中的`title`变量可以用<code>&#123;&#123; page.title &#125;&#125;</code>访问到。也就是说，站点的全局变量在\_config.yml中定义，用`site.`访问，页面的变量在[YAML Front Matter][YFM]中定义，用`page.`访问，更多的模板变量可参考[模板数据][TD]。

\_includes/JB中的插件可以在\_includes/custom中重新自定义，方法可仿照\_includes/JB中的。如果要\_includes/custom中的插件生效，需要在\_config.yml中将相应的变量设为`custom`，设置线索可见\_includes/JB中的插件。

### 小结
[jekyll生成静态页面的过程](http://jekyllbootstrap.com/lessons/jekyll-introduction.html#how_jekyll_generates_the_final_static_files)就是先搜集站点的原始数据，通过计算，生成用于页面显示的结构化数据（最终的html静态页面）。结构化数据来源有两方面：一方面是文件中的配置文件，如\_config.yml和markdown的[YAML Front Matter][YFM]；另一方面通过目录结构和jekyll定义的解析法则，产生数据。站点的全局数据用`site.`访问，页面数据用`page.`访问。有了这些结构化的数据后，jekyll再按照用户定义的模板，用相应的结构化数据替换模板中的变量。用户编写的插件是为了计算出满足用户特定需求的数据。

##装饰部件

###图片与文件


###代码高亮
网页代码高亮的一般原理是先用JS对代码的进行解析，并提根据不同程的序语言提取关键字、变量、常量、注释，然后将代码层次结构用html标签描述，最后用CSS着色。

代码高亮的方案有很多，比如：[转帖gist代码的插件](https://gist.github.com/1027674)，[google-code-prettify](http://google-code-prettify.googlecode.com/svn/trunk/README.html)，[利用pygments高亮代码的插件](https://github.com/rsim/blog.rayapps.com/blob/master/_plugins/pygments_cache_patch.rb)，octopress的[backtick-codeblock](http://octopress.org/docs/plugins/backtick-codeblock/)，如果需要在线展示html、js、CSS及其效果，当然还是用[jsfiddle插件](http://octopress.org/docs/plugins/jsfiddle-tag/)。

利用pygments，须先在\_config.yml中设置`pygments: true`，并嵌入生成的相应CSS。   
{% highlight bash %}
$ pygmentize -S default -f html | sed 's/^/.highlight code /g' > default.css
{% endhighlight %}   
在pygments的CSS选择器前都加上`.highlight code`，防止pygments的CSS影响[mathjax](#mathjax)公式的CSS。[pygments也可能会和bootstrap.min.css冲突](http://www.stehem.net/2012/02/14/how-to-get-pygments-to-work-with-jekyll.html)，需要修改css。上面的`pygmentize`命令就是pygments代码高亮的效果。

转帖gist代码的效果是这样的：
{% gist 834610 %}

另一个比较特殊的代码高亮插件是[include-code](http://octopress.org/docs/plugins/include-code/)，可以直接显示目录中的代码文件，在\_config.yml中设置好`code_dir`参数后，直接用<code>&#123;% include_code excerpt.rb %&#125;</code>，即可显示高亮代码（布局是自己定义的，代码高亮的CSS和pygments一样）如下：  
{% include_code excerpt.rb %}

<span id="mathjax"></span>


