---
layout: post
title: 使用Emacs编辑markdown文件
tags: 
- emacs
- markdown
status: publish
type: post
published: true
---


陆陆续续尝试使用Emacs已经该有7、8回了，每次都受不了它繁琐的Ctrl和Meta组合键，但最近不知道哪根筋出了问题，不但编辑器迅速的切换到了Emacs，甚至
操作系统也从Win 7平滑到了Ubuntu。以下记录一些关于Emacs、markdown、Ubuntu、ssh乱七八糟的东东。

开源体系下，我的软件之路差不多是下面这样的：

> R->LaTeX->imagemagic->Emacs->Ubuntu->github(git,svn)->markdown->pandoc->putty

当走到putty这一步，基本上也能称之为半个合格的码农了，囧。

Emacs是非常好用的文本编辑器，是著名黑客stallman的作品，同vi并称为linux体系同两大神器。用它来编辑任意文本有大量的定制扩展，试用起来非常方便，而最近老板也在推行用markdown来记录技术文档，并且在内网构建了基于 markdown 的 git wiki 系统。刚好自己也一直在用md，比如现在的这个搭建在github上的静态页面博客。

# Emacs 的安装

在ubuntu下安装Emacs非常简单，甚至可以一起装好ess：

	sudo apt-get install emacs ess

在Emacs里运行R还是很爽的事情。

## Emacs 的设置文档

主要支持语法高亮，以及一些人性化的配置，不多说直接贴.emacs [文件](/upload/emacs)（改名为`.emacs`，在/home/user目录，可能需要Ctrl+h显示隐藏文件）

这里面有个emacs的颜色主题，用于代码高亮，需要下载color-theme.el，请自行搜索之，并拷贝至emacs可以找到的目录下。如果不许要注释掉相关行即可。

## 安装markdown-mode和pandoc

下载[markdown-mode.el](http://jblevins.org/projects/markdown-mode/)至 emacs 的load path，并编辑.emacs增加以下内容，保证识别后缀名为text、markdown、md文件：

	(autoload 'markdown-mode "markdown-mode"
		"Major mode for editing Markdown files" t)
	(add-to-list 'auto-mode-alist '("\\.text\\'" . markdown-mode))
	(add-to-list 'auto-mode-alist '("\\.markdown\\'" . markdown-mode))
	(add-to-list 'auto-mode-alist '("\\.md\\'" . markdown-mode))

接着安装pandoc（用于将markdown文件转化为html文件）

	sudo apt-get install pandoc

markdown的那个程序也可以类似的安装，但不能像pandoc一样增加各种参数设置，所以不建议大家使用。

## 修改markdown-mode 的Customization

一般是通过 `M-x customize-mode` 来修改，将Markdown Command 修改为

	pandoc -s -c /home/sunbjt/emacs/style.css --mathjax --highlight-style espresso

（或者是修改.emacs 文件）
上面这句话有两个位置需要注意：

- style.css，可以自定义你的生成的html文件的样式
- mathjax参数，在C-c C-c p做浏览器预览时，可以自动加载数学公式

接着是几个组合命令：

- C-c C-c v 和 p一样，是对 md 文件的 browser 端的预览；
- C-c C-c e，重新刷新已打开的预览；
- C-c C-c m，在buffer里看html的源代码

对于css文件，我自己常用的在[这里](/upload/style.css)，基本上是仿github的风格。如果不是在Emacs下书写markdown，可以通过在命令行执行如下代码，以生成自定义的html文件：

	pandoc -s doc.md -c style.css -o report.html

# 翻呀啊墙

为保险起见，请先执行这段R代码：

	id <- c(7, 15,  1,  7,  5, 14, 20)
	paste(letters[id], collapse='')
	
你要用上面那个软件，教程一大堆，唯一需要注意的是Windows和Liunx的放在了一起，只要有Python环境，执行即可穿越。

如果使用商用的ssh，那么用 expect 自动登陆ssh，

	sudo apt-get expect

然后再写一个自动执行登陆的脚本：

	#!/usr/bin/expect
	set timeout 60
	spawn /usr/bin/ssh -p 80 -D 7070 -g username@8.8.8.8
	expect {
	"password:" {
	send "password\r"
	}
	}
	interact {
	timeout 60 { send " "}
	}

就这样，全部工作都在Ubuntu下了~~


# 其他

ubuntu 平台下 Courier New 字体

	apt-get install ttf-mscorefonts-installer 

Google 拼音

	sudo apt-get install ibus-googlepinyin
