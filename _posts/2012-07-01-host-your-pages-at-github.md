---
layout: post
title: "github 建站指南"
description: ""
category: 振导社会
published: true
tags: [github, git, jekyll, 速查, 简介, 程序]
---
{% include JB/setup %}

简介：本文主要介绍利用jekyll在github上搭建站点。

##github驱动网站原理

github提供了[自动](https://help.github.com/articles/creating-pages-with-the-automatic-generator)和[手动](https://help.github.com/articles/creating-project-pages-manually)建站的文档。github的网站是位于`username.github.com`仓库中的静态页面。因此，利用高效的工具生成高质量的静态页面是github建站的王道。[jekyll](http://jekyllrb.com/)就是用[ruby](http://www.ruby-lang.org/)制造的这样的利器，但是，构建网站ruby不是必须的。模板、数据和美化是jekyll建站的三要素。jekyll自动将数据注入模板，通过美化，得到站点。这里可以看到[一堆用jekyll打造的站点](https://github.com/mojombo/jekyll/wiki/Sites)，大部分都可以`git clone`回来学习参考。

###jekyll的模板
jekyll模板就是带有变量的html格式文件。jekyll的模板用[Liquid](http://liquidmarkup.org/)语言描述，定义了页面框架。Liquid通过（Output）和标签（Tag）两种标记（markup），即可描述页面的结构，具体可参阅[Liquid for Designers](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers)。jekyll对Liquid的标签进行了[扩展](https://github.com/mojombo/jekyll/wiki/Liquid-Extensions)。Liquid模板语言对模板数据描述（当然也需要html），生成了站点页面的骨架。Liquid所需要的jekyll的[模板数据][TD]在这儿可以找到。除此之外，[YAML Front Matter][YFM]定义的变量也可以作为模板数据。

[TD]: https://github.com/mojombo/jekyll/wiki/template-data   
[YFM]: https://github.com/mojombo/jekyll/wiki/YAML-Front-Matter

###jekyll的数据
jekyll的数据是用[markdown](http://daringfireball.net/projects/markdown/)/[textile](http://textile.sitemonks.com/)写的文档。markdown等纪录的内容转换为html文档，注入jekyll的模板的 &#123;&#123; content &#125;&#125; ，就有了网站的页面。jekyll支持的markdown解析器（将markdown转换为html）有：<span id="markdown"></span>[rdiscount](https://github.com/rtomayko/rdiscount/)、[kramdown](http://kramdown.rubyforge.org/)、[redcarpet](https://github.com/tanoku/redcarpet/)、[maruku](http://maruku.rubyforge.org/)（jekyll默认）、[bluecloth](http://deveiate.org/projects/BlueCloth/)。为了方便处理LaTeX公式，也有人hack了jekyll，[将pandoc作为markdown的解析器](http://yangzetian.github.com/2012/04/15/jekyll-pandoc/)。

###jekyll的美化
美化就是用css和javascript对html描述的页面进行渲染，美化网页。[twitter bootstrap](http://twitter.github.com/bootstrap/)是一套极易上手的页面美化工具。twitter bootstrap还提供了[960网格布局](http://960.gs/)，只要按照它约定的方式对页面结构定义、对html标签的`class`命名，网页的布局美化可谓快又好。

##在jekyll-bootstrap基础上搭建网站

jekyll只是定义了一套github生成网站的规程，并且提供了相应的工具。但是，按照这套法则，从头开始仍然麻烦。[jekyll-bootstrap](http://jekyllbootstrap.com/)就是一个jekyll构建站点的demo，在此基础上学习修改，相当快捷容易。

jekyll-bootstrap的[目录结构](https://github.com/mojombo/jekyll/wiki/usage)中，只需要将markdown文档放到`_posts`目录下，`jekyll --server`，即可在`http://localhost:4000`看到页面内容，见[快速入门示范](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)。`_posts`中的数据文档，通过注入`_layouts`定义的模板（`_layouts`的模板一般指向了`_includes/themes`中的模板），渲染页面的css和js文档在`assets/themes`中，一些解析markdown用到的ruby插件放在`_plugins`目录。通过`jekyll --server`最终生成的静态页面在`_sites`目录，但是，在更新站点的时候`git push`推送的是其上层目录，而不需要`_sites`目录，页面的解析留给github做了。

jekyll-bootstrap的`_includes/JB`中有一些常用的工具，用于列表显示、评论等，可参看`_includes/themes`中的html文档学习。

`_includes/themes`中的主题一般包含default.html、post.html和page.html三个文档。default.html定义了网站的最上层框架，post.html和page.html是其子框架。`rake page`生成的页面按page.html解析，markdown文件的[YAML Front Matter][YFM]中`layout: page`；`rake post`生成的页面按post.html解析，markdown文件的[YAML Front Matter][YFM]中`layout: post`。生成好的html页面再会替换default.html文档的 &#123;&#123; content &#125;&#125; 变量，这样才完成了整个页面的生成。

站点生成需要用到`_config.yml`[配置文件](https://github.com/mojombo/jekyll/wiki/configuration)，`markdown`定义了解析markdown用的[解析器](#markdown)。当然也可自定义变量，比如在`JB:`下定义一个二级变量`RSS_path: /atom.xml`（注意缩进）。在html页面中，就可以用`{{ site.JB.RSS_path }}`得其值。[YAML Front Matter][YFM]中也可以自定义变量，其中的`title`变量可以用`{{ page.title }}`访问到。也就是说，站点的全局变量在`_config.yml`中定义，用`site.`访问，页面的变量在[YAML Front Matter][YFM]中定义，用`page.`访问，更多的模板变量可参考[模板数据][TD]。

`_includes/JB`中的插件可以在`_includes/custom`中重新自定义，方法可仿照`_includes/JB`中的写。如果要`_includes/custom`中的插件生效，需要在`_config.yml`中将相应的变量设为`custom`，设置线索可见`_includes/JB`的插件。

##装饰部件
###插入图片
###代码高亮
###LaTeX公式

