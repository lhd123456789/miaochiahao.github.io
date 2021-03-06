---
layout: post
tags: SQL注入 渗透测试
title: "SQL注入（一）"
category: SQL注入
excerpt: SQL注入原理、注入点的判断及简单防护
---

上次在蓝盾的时候第一次接触到了SQL注入和SQLmap工具，想着考完以后系统的学一下SQL注入。这个系列的博文都是在i春秋学习的笔记和自己实际操作的过程。

## SQL注入原理
程序命令与用户输入之间没有做到泾渭分明，使攻击者有机会将命令通过用户输入提交给Web程序，从而进行攻击 
注入攻击的最终是数据库，与平台和脚本无关
 
## SQL注入前
* 确定Web应用程序使用的技术
* 确定可能的输入方式（burp等）
* 查找可以用于注入的用户输入

## SQL注入点的判断

### 网页链接特征


>* http://www.******.com/***.asp?id=xx (ASP注入)
>* http://www.******.com/***.php?id=xx (PHP注入)
>* http://www.******.com/***.jsp?id=xx (JSP注入)
>* http://www.******.com/***.jspx?id=xx （JSPX注入）
>* http://www.******.com/***.asp?id=xx&page=99 （有两个参数，注入时要注意参数的确认）
>* http://www.******.com/index/new/id/8 (伪静态)
>* http://www.******.com/index/new/php-8.html (伪静态)


### 判断是否存在注入漏洞
* 单引号法：在链接后加上一个单引号，若页面不能正常显示，浏览器返回异常信息，可能存在漏洞
* 1=1与1=2法：在链接后分别加上“and 1=1”和"and 1=2"进行提交，若返回不同页面，可能存在漏洞

## 防止注入
简单来说只需要在 `?id=xx` 这里判断下输入进的 `xx` 是否为纯数字即可，若不是则拒绝执行。
对于PHP

{% highlight ruby %}
if (!is_numeric($id)){
	echo "防止sql注入"
	echo "<hr>"
}
{% endhighlight %}

通过防止注入方法，虽然可以判断出存在SQL注入漏洞，但是不能进一步查询