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
jekyll的数据是用[markdown](http://daringfireball.net/projects/markdown/)、[textile](http://textile.sitemonks.com/)等标记语言写的文档。这些格式的文档解析为html格式后，注入jekyll模板的<code>&#123;&#123; content &#125;&#125;</code>，就有了网站的页面。jekyll支持的markdown解析器（将markdown转换为html）有：<span id="markdown"></span>[rdiscount](https://github.com/rtomayko/rdiscount/)、[kramdown](http://kramdown.rubyforge.org/)、[redcarpet](https://github.com/tanoku/redcarpet/)、[maruku](http://maruku.rubyforge.org/)（jekyll默认）、[bluecloth](http://deveiate.org/projects/BlueCloth/)。为了方便处理{% m %}\LaTeX{% em %}公式，也有人hack了jekyll，[将pandoc作为markdown的解析器](http://yangzetian.github.com/2012/04/15/jekyll-pandoc/)。一个更方便的方法是通过[jekyll-pandoc-plugin](https://github.com/dsanson/jekyll-pandoc-plugin)插件，启用pandoc解析器。但是，这些hack后的jekyll启用pandoc不会在github上生效，只能用于本机。








