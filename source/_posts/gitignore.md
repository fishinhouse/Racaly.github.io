---
title: git忽略
tags: git
date: 2016/9/04 
---
### .gitignore
 1.在项目的根目录下打开.gitignore文件（如果没有则新建），添加你要忽略的文件或者文件夹，例如： *.json,theme/
 2.清除缓存
{% codeblock  %}
	git rm -rf --cached .
	git add .
{% endcodeblock %}

3.这时修改的.gitignore就生效了。