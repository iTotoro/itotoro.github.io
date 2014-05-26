---
layout: post
title: "Jdbc连接AS400执行insert等操作遇到的问题[SQL7008]"
description: "use db2 Jdbc connect AS400 to insert data get SQL7008 issue"
category: issue
tags: [AS400,SQL7008]
---


最近接手一个后端数据源更换的项目，把原来的存储介质SAP转到AS400上。本以为还算一个不是很麻烦的过程，只要更改DAO层就可以了。。。

不过做了之后还是发现有些问题需要解决，由于AS400数据源由客户控制和管理，所以很多刚开始就碰到很多小问题，列名对不上等等一系列问题。

###	1. 表的列名问题
	   	
执行SELECT查询的时候，显示的列名并不是真正的column name，可以使用 `SELECT COLUMN_NAME,DATA_TYPE FROM COLUMNS WEHERE TABLE_NAME='XXXXX'`获得具体的列名和字段类型。

###	2. 表的类型	
		
表居然分为物理表和逻辑表，执行INSERT的时候只能插入到逻辑表。

###	3. 表的主键
	
表可以设置主键，但是可以插入两条一样的记录，这点让我很费解啊。而且主键不能自动增长，这样我们只有自己生成主键，产生主键又是一个问题？[多用户同时操作就多了一个并发问题]

###	4. 	事物自动提交和手动提交

因为刚开始测试用了`autocommit`来自动提交，没问题，可以插入记录，然后一个事物用手动提交来测试，出现异常，提交失败。

这个时候只有请教谷哥来帮忙了。原来需要为这个表创建一个日志接收器。

创建日志接收器：[Create Journal Receiver (CRTJRNRCV)](http://pic.dhe.ibm.com/infocenter/iseries/v7r1m0/index.jsp?topic=%2Fcl%2Fcrtjrnrcv.htm)

具体问题没去深究，先解决问题再说。


好歹两三年前哥也是培训了四个月的mainFrame的啊，时间真的会冲淡很多东西，如果你不去经常回顾的话。

