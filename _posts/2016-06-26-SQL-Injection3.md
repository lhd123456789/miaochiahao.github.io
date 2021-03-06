---
layout: post
tags: SQL注入 渗透测试
title: "SQL注入（三）"
category: SQL注入
excerpt: MySQL数据库常见注入技术以及不同版本数据库具体情况分析
---
> 如果觉得这篇文章写的不错，来给我[打赏鼓励](https://github.com/miaochiahao/miaochiahao.github.io/blob/master/pictures/alipay.jpg?raw=true)一下吧~我会写出更好的文章

## MySQL数据库常见注入攻击技术

### MySQL数据库情况

在MySQL 4及之前版本，由于不支持子语句查询且配置文件“php.ini”配置文件中的magic_quotes_gpc参数设置为“On”时（默认开启），提交的变量中包含单引号，双引号，反斜线，and和空字符时，会被数据库自动转换为含有反斜线的转义字符。
在MySQL 5版本数据库中，新增加了information_schema数据库，该库中储存了数据库信息内容，可以直接进行爆库、爆表、爆字段，攻击变得更简单。

### MySQL 4注入攻击方法

1. 利用 `order by` 获得当前表的字段数
2. 使用 `union select` 获取想要的数据库信息
3. 由于不知道数据库中的表名和字段名，只能像Access一样使用常见表名和字段名进行猜解(啊D)

### MySQL 5注入攻击技术

* 通过对MySQL的数据进行猜解获得敏感的信息，进一步获得网站控制权
* 通过 `load_file()` 函数来读取脚本代码或系统敏感文件内容，进行漏洞分析或直接获取数据库连接账号、密码
* 通过 `dumpfile/outfile` 函数导出获取WebShell

## 手工注入攻击

### 注入点信息检测

* 单引号判断法
* `1=1和1=2`判断法

### 注入类型判断


#### 注入类型

* 数字型注入 `select * from news where id=$id`
* 字符型注入 `select * from news where username='$name'`
* 搜索型注入 `select * from news where password like %best%`

#### 注入方法

* 数字型注入 `select * from news where id=$id `此处后面可以直接加语句 `and 1=1`
* 字符型注入 `select * from news where username='$name'` 此处应当闭合引号 `' and 1=1 '`
* 搜索型注入 `select * from news where password like %best%` 此处应当闭合引号和百分号`%' and 1=1 '%`

### 注入点提交方式判断

* GET提交：可通过地址来进行注入
* POST提交：上传文件时、用户名密码登录时进行抓包（Burp），在Burp中进行手注测试；有时POST提交也会接受GET提交方式，可以通过补全地址的方式来进行测试
* Cookie提交：抓包后可修改测试


## MySQL相关语法

`order by n` 此语法意为按照第n个字段进行排序，n应从1开始一直试验到报错。目的是用这种方法来判断字段数
`union select` 联合查询
