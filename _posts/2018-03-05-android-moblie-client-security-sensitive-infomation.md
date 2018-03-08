---
layout: post
title: Android 客户端安全---敏感信息安全测试
date: 2018-03-09 19:33:10 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: mac.jpg # Add image post (optional)
tags: [pentest]
---

## 敏感信息安全
检测客户端是否会保存明文敏感信息，是否对保存的敏感信息进行恰当加密，能否防止用户敏感信息的非授权访问。
android应用的数据文件夹为“／data／data／应用名称／”，如无具体说明，下面测试的文件都位于此文件夹。
本地文件敏感信息检测（低风险）
有的开发者在开发软件的时候，安全意识不够强，或者为了降低开发难度，将一些敏感信息直接明文保存在App的相关文件中，攻击者可以通过获取这些文件获得一些如密码、token等敏感信息。
测试步骤：
1．	拉取/data/data/com.zaful/目录下的内容到PC逐一查看分析
 
测试结果：
不安全。文件中泄露了敏感信息。
修复建议：
敏感信息加“*”后保存在本地
##Logcat日志敏感信息检测（安全）
程序员在开发过程中非常喜欢使用Log函数输出日志，可以在运行时候通过logcat和DDMS进行查看方便调试。其中这些日志中就有可能有用户名密码等重要信息，如果手机中了木马，那么木马就可以通过收集日志，获得用户的敏感信息。
测试步骤：
1．	登录zaful，使用其中的各种功能。
2．	logcat查看手机日志。
 
测试结果：
安全。在日志中未发现用户密码等敏感信息，
##本地SQL注入检测（安全）
Android应用使用content provider将数据存储层和应用层分离，为程序在应用程序内核应用程序之间存储、共享和使用结构化数据提供相应的操作。如果应用存在的暴露provider，并且未对传入content provider的查询语句做过滤的话，攻击者可以使用SQL注入的方法读写应用数据。
测试步骤：
使用drozer获取注入点，对注入点尝试注入。
 
测试结果：
安全。如图所示，有暴露的provider，但是不能进行注入。
