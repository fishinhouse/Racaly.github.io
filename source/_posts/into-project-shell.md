---
title: 制作一个进入项目目录的小脚本
tags: shell
date: 2016/7/13 
---

### 需求 
&emsp;&emsp;在我的项目文件夹里有四个不同的项目，/app/project/,项目文件夹分别为testa，testb，testc，testd。每次启动ubuntu后我要进入第一个项目就需要运行cd app/project/testa,git statua 。所以需要一个自定义的命令可以是我直接进入指定的项目目录，并执行一次git status。因为我是希望根据我传的参数进入指定的目录，会需要一些判断。所以单纯使用alias有点不太妥当，这里就使用alias 和shell 脚本来实现这个功能。


### 写一个简单的shell脚本

1.新建一个into_project.sh文件：
	{% codeblock  %}
	#!/bin/bash
	#声明一个关联数组并赋值
	declare -A project
	project[a]=~/app/project/testa/
	project[b]=~/app/project/testb/
	project[c]=~/app/project/testc/
	project[d]=~/app/project/testd/ 
	{% endcodeblock %}
&emsp;&emsp;至于什么叫关联数组或者关联数组如何定义，请自行google搜索shell脚本的关联数组，在这里使用关联数组是因为需要下标可以是字符串的数组。然后给数组赋值

2.完整代码，实现功能
	{% codeblock %}
	#!/usr/bin/
	#项目目录
	declare -A project
	project[a]=~/app/project/testa/
	project[b]=~/app/project/testb/
	project[c]=~/app/project/testc/
	project[d]=~/app/project/testd/
	falg=false     #flag默认为false
	if [ x$1 != x ]; then      #$1为传进来的参数，这里判断它是否为空
		for key in ${(k)project}; do     #循环这个关联数组的key
			 
			if [ "$1"x = "$key"x ]; then   #判断传进来的参数是否和里边的其中一个相等
				falg=true   #如果相等，flag为true
				break   #结束循环
			fi
		done
		if $falg; then     #如果falg为true 执行这里边的内容
			echo "您已进入$project[$1]项目" 
			cd $project[$1]      #进入刚才找到的那个目录
	  	git status      #运行git status
	  else    #否则，根据传进来的参数没找到对应的项目
	  	echo "没有这个项目，项目列表：" 
	  	for key in ${(k)project}; do printf "名称:%-5s %-6s\n" $key $project[$key]; done    #遍历project数组并输出，以提示
	  fi
	else  #如果$1为空，没有传参数
	    echo "请指定一个项目！项目列表："
	    for key in ${(k)project}; do printf "名称:%-5s %-6s\n" $key $project[$key]; done     #遍历project数组并输出，以提示
	fi
	{% endcodeblock%}
### 定义自定义命令 
1.测试
&emsp;&emsp;现在需要一个命令来调用这个脚本，当然，可以先测试一下。
	{% codeblock %}
	username$ /bin/bash into_project.sh a
	{% endcodeblock %}
&emsp;&emsp;输出如下：
>   username$
	您已进入/home/***/project/testa/项目
	位于分支 master
	您的分支与上游分支 'origin/master' 一致。
	无文件要提交，干净的工作区
	username$

可以发现，脚本运行成功了，但是当前shell并没有进入指定的文夹里，因为运行into_project.sh时，会新启一个shell脚本，执行完毕后关闭，所以需要这么执行：
	{% codeblock %}
	username$ source into_project.sh a
	{% endcodeblock %}
&emsp;&emsp;输出如下：
>   username$
	您已进入/home/***/project/testa/项目
	位于分支 master
	您的分支与上游分支 'origin/master' 一致。
	无文件要提交，干净的工作区
	username:~/project/testa/$
2.定义命令
&emsp;&emsp;ubuntu可以使用alias来将现有的命令自定义，相当于起了一个别名
如**alias cl = "cd project/texta"**.alias 根本就不支持传递命令行参数。不过，我们可以变通的解决这个问题，即定义函数。
 {% codeblock %}
 into='into() { source /home/zy/into_project.sh ;}; into'
 {% endcodeblock %}
&emsp;&emsp;但是这个命令在用户关闭shell进程后就失效了。我用的是oh-my-zsh，所以打开**~/.zshrc文件，将刚才的命令添加进去，重启或者source ~/.zshrc,这样这个命令就可以使用了