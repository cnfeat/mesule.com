--- 
layout: post
title: ubuntu 下编写 R 脚本的利器-Gedit
tags: 
- Gedit
status: publish
type: post
published: true
---
<p>用于编辑 R 脚本的文本编辑器很多，个人更倾向于使用开源编辑器（关于 R 的编辑器我在 <a href="http://cran.r-project.org/doc/contrib/Liu-FAQ.pdf" target="_blank">RFAQ_cn</a> 上也提过）。这里主要介绍一下 <a href="http://en.wikipedia.org/wiki/Gedit">gedit</a>，一款在 GNOME 桌面环境下的文本编辑器。</p>
<p>虽说 geidt 看起来就和 Windows 下面的 notepad 一样简陋，但它对于运行一般 R 脚本已经足够用，因为 gedit 编辑器可以将 R 代码直接作为外部命令运行。具体实现方式如下：</p>
<p>依次选择：编辑-首选项-插件-外部工具（执行外部命令和Shell脚本）</p>
<p><img alt="Screenshot-外部工具管理器.png" height="344" src="http://bjt.cos.name/wp-content/uploads/2009/07/xn-screenshot_-zb7r372cm0iuxs0q7fx90aib4f.png" width="480" /></p>
<p>新建一个工具R，在命令中写入 R --no-save --no-restore -q，同时写入描述和快捷键（这里定义的是ctrl +r）。</p>
<p>输入可选择&quot;选中区域&quot;和&quot;全选&quot;，输出的话，&quot;在下方面板中显示&quot;比较好一些，当然这个看个人喜好。</p>
<p>编辑 R 代码过程中，选中要执行的代码，ctrl+r，OK，输出结果，非常方便。</p>
<p><img alt="Screenshot-新文件 (~-桌面) - gedit.png" height="526" src="http://bjt.cos.name/wp-content/uploads/2009/07/xn-screenshot_________gedit-q632bq596av5au84cvf63a.png" width="650" /></p>
