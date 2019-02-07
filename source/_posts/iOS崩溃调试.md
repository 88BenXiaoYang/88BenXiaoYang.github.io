---
title: iOS崩溃调试
date: 2019-02-07 10:56:54
tags: iOS
categories: iOS
---

根据`.ips`文件进行分析。

* 准备

```
新建一个临时文件夹，将.ips文件更改为后缀为.crash的文件放入临时文
件夹，找到.ips文件对应的.xcarchive文件，查看内容找到.app
和.dSYM文件，都放入临时文件夹。
```

* 校验

**需要校验.ipa，.crash(.ips)，.dSYM的一致性。**

方法：通过验证构建版本的UUID和crash信息包含的UUID是否一致。

步骤：

```
1.查看.crash日志中包含的UUID
cd到.crash所在文件夹，使用指令：
grep --after-context=2 "Binary Images:” xxx.crash
（Binary Images:为.crash文件中的特定字符串，context=2取Binary Images:后的前两行内容）
<>中的内容即为UUID

2.查询.ipa的UUID
解压.ipa文件（将原来.ipa文件后缀改为.zip解压出Payload文件夹），cd到解压后的文件夹，使用指令：
xcrun dwarfdump --uuid xxx.app/xxx

3.查询.dSYM文件的UUID
找到.xcarchive（显示包内容）中的.dSYM文件，使用指令：
dwarfdump --uuid （.dSYM所在路径）
```

* 将崩溃日志符号化

使用xcode自带的符号化工具（两种方式）：

```
方式一：
1.1: 找xcode自带的符号化工具（symbolicatecrash）：
find /Applications/Xcode.app -name symbolicatecrash -type f
将查询结果中/Applications/Xcode.app/Contents/SharedFrameworks/DVTFoundation.framework/Versions/A/Resources/symbolicatecrash该路径下的symbolicatecrash工具，
放入临时文件夹，此时临时文件夹中有：symbolicatecrash，.crash，.app，.dSYM。

1.2: 在终端cd到临时文件夹：
先export DEVELOPER_DIR=/Applications/Xcode.app/Contents/Developer，
再./symbolicatecrash xxx.crash xxx.app > xxx.log，执行完该命令后，会在临时文件夹中生成xxx.log文件（即为崩溃日志符号化后的文件）

方式二：
临时文件夹中只有.crash，.app，.dSYM时，
先export DEVELOPER_DIR=/Applications/Xcode.app/Contents/Developer，
再/Applications/Xcode.app/Contents/SharedFrameworks/DVTFoundation.framework/Versions/A/Resources/symbolicatecrash xxx.crash xxx.app > xxx.log，执行完该命令后，
会在临时文件夹中生成xxx.log文件（即为崩溃日志符号化后的文件）
```