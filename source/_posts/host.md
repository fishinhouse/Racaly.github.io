---
title: Chrome浏览器host配置
tags : 其他
date: 2016/10/13 
---
### Chrome浏览器host配置
#### 一、host文件配置
##### Windows平台
1.使用文本编辑器打开： %SystemRoot%\System32\drivers\etc\hosts
2.将 [hosts](https://raw.githubusercontent.com/racaljk/hosts/master/hosts) 全部内容复制到上面的文件内并保存
##### Linux系统
1.将 hosts 全部内容复制到/etc/hosts中并保存
2.或者在终端执行：
{% codeblock  %}
wget https://raw.githubusercontent.com/racaljk/hosts/master/hosts -qO /tmp/hosts && sudo sh -c 'cat /tmp/hosts > /etc/hosts' 
{% endcodeblock %}
#### 二、Chrome配置
1.地址栏输入：`chrome://net-internals/#hsts`,回车
2.` Input a domain name to add it to the HSTS set: `这一行下面的`` Domain: [ ]``输入`` www.google.com.hk ``点 **Add**按钮

#### 三、重启浏览器

[原文](https://github.com/racaljk/hosts)及相关问题 请访问`https://github.com/racaljk/hosts`