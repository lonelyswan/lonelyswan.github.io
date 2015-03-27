---
layout: post
title: iterm+zsh＋powerline＝OMG
description: "在Yosemite里给iterm的zsh配置powerline theme"
modified: 2015-03-29
tags: [zsh,powerline]
image:
  feature: abstract-3.jpg
---

看够了zsh的老主题robbyrussell，换个亮眼的powerline主题享受下。

自从用了zsh觉得terminal原来可以如此美好。iTerm的强大功能也让我欲罢不能啊。加上PowerLine这么酷炫的主题，简直爽翻。先上图。
<figure>
<img src="https://raw.githubusercontent.com/lonelyswan/lonelyswan.github.io/master/images/powerline.png" alt="">
</figure>
颜色清爽，还有git的分支图案。是不是蛮爽。

###跟我一块一步一步配置下吧。

首先你当然要安装zsh和iTerm了。这个就不多说了。

* 下载主题安装
{% highlight css %}
git clone git://github.com/jeremyFreeAgent/oh-my-zsh-powerline-theme ~/.ohmyzsh-powerline
cd ~/.ohmyzsh-powerline
./install_in_omz.sh
{% endhighlight %}
	
* 因为powerline主题用到了一些特殊的符号所以需要下载一个支持powerline的字体，如果你喜欢mac命令行自带的字体那么我推荐下载[Monaco for Powerline](https://github.com/supermarin/powerline-fonts/blob/bfcb152306902c09b62be6e4a5eec7763e46d62d/Monaco/Monaco%20for%20Powerline.otf)字体。双击下载下来的字体文件点击安装字体就可以了

* 给zsh配置主题
{% highlight css %}
vim ~/.zshrc 
找到ZSH_THEME修改为powerline
保存后
source ~/.zshrc
{% endhighlight %}

* 打开iTerm，

