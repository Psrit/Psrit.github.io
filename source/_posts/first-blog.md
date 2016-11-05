---
layout: '[post]'
title: First Blog
date: 2016-10-30 20:46:46
tags:
---
Hello World! 

Well, as a "little white", I don't know what to say... 

Emm... I think, the font is really ugly; the theme "huno" doesn't perform well on mobile devices. 

Also, I found that emoji was not supported... (although which is not something matters)

中文倒是可以。

（上一篇是Hexo自带的，就作为教程搁这儿吧……一个更好的教程可以[看这里](http://www.tuicool.com/articles/AfQnQjy/)。

作为还有三天期中考试的大四狗，我得滚去复习了……考完再弄……）



[2016-11-05]

考完C++回来看了一下博客，发现智能的 Typora 其实在幕后帮我做了很多事情……

但是没有它之后就无所适从了。目前发现的 hexo markdown 渲染引擎和 Typora 不兼容的地方包括：

- $\\mathrm{\\TeX}$ 数学公式

  在 Typora 里，像在 .tex 里输入一样直接写 TeX 代码就可以生成想要的公式；

  在原始的 markdown 里，像`\`，`.`，`_`等都被视为内部标识符。为了能正常使用 TeX 语法，需要对所有这些操作符用`\`转义，如表示换行的`\\`应写成`\\\\`（其实所有地方都要转义，只是数学公式这里有点出乎我的意料）。在 Code 环境里，不用管`\`。

  所以，朕现在只好先用 Typora 写好，再用 Sublime 打开逐个修改（转义后的 TeX 代码是不能被 Typora 解释的，所以在 Typora 里直接修改会显示错误，简直不堪入目）…………等学会了 Python，我想可以写个小脚本以替代人力。

- 表格

  Typora 支持列表内的表格，这种表格和列表项具有相同的缩进。体现在源码里就是每一行开头的`|`都会有一个缩进（缩进量我忘了记录了……）。

  而原始 markdown 好像不支持这种表格每行开头的缩进，渲染出来后就直接是源码的样子……把所有这类缩进全部删掉就没问题了。至于问题的根源是和*有无缩进*有关还是和*缩进格式*（见下面的“代码”一项）有关，尚有待考证……

- 代码

  Typora 代码环境里，一个 Tab 等于八个空格。而且输入多少空格，它就会忠实地输出多少空格。

  然而看源码会发现，Typora 的缩进格式可能会比较奇怪，里面可能同时混杂有 Tab 和空格（我也不知道为什么……可能是因为代码是别处复制过来的，那里用的是若干空格作为缩进；复制到本地文件后自己再加入了 Tab 啥的……），这样渲染出的格式参差不齐。所以只好把所有缩进都删掉，然后用 Tab 实现（Markdown 代码环境里两个 Tab 渲染出来是4个空格）。

（其实我觉得以上一些奇怪的 bug 和最近鼠标左键出了些问题不无关系……因为想用左键选取的时候老是莫名地拖动甚至删除想选的文字。

Whatever, 滚去复习信号与系统了）