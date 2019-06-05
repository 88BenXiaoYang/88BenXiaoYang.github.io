---
title: iOS工具手册
date: 2019-01-09 09:24:17
tags: iOS
categories: 移动端
---

##### `icon`尺寸

```
29*29，57*57，
58*58，80*80，
87*87，114*114，
120*120，180*180
```

##### 导航图/启动图尺寸

```
640*960     
640*1136
750*1334    
1242*2208
1125*2436
```

##### `Xcode`中描述文件所在位置

`~/Library/MobileDevice/Provisioning Profiles`

##### IMEI号查询

`拨号：*#06#`

##### `Xcode`中的文件夹

```
Xcode中文件夹（物理文件夹、逻辑文件夹）的图标显示和逻辑控制受.DS_store文件的影响。
```

##### `dSYM`

```
dSYM：
dSYM是在打包过程中生成的保存app函数地址映射信息的中转文件。
(对应的函数地址是16进制表示)
使用dSYM的场景：
通过出错的函数地址去查询dSYM文件中程序对应的函数名和文件名，从而得到错误详细信息。
```

##### 开发中常用加密流程

```
加解密双方约定好“加盐”的字符串
iv = md5("")
key = md5("")

约定好参数的排序规则
//para->query-string->sorded
qureystring = "k1=v1&k2=v2" (sort by key)

使用加密算法对请求参数字符串进行加密
//encrpt
enc_value = BASE64_URLSAFE(AES128(key, iv, qureystring))

将加密后的请求字符串拼在接口IP后面生成最终的请求url
//final-url
http://xxxx.xxxx.aaa?sid=enc_value

示例一：
参数串= param1=value1&param2=value2...
加密串= BASE64(AES128(参数串, MD5(AES_KEY), MD5(AES_IV)), URL_SAFE)

示例二：
参数串= JSON 字符串
加密串= BASE64(AES128(参数串.utf8, MD5(AES_KEY), MD5(AES_IV)), URL_SAFE)
```

##### `Watchdog Timer`

```
在Xcode中运行代码时，Watchdog Timer是禁用的，
以补偿OS X系统中的各种开支，如调试器等。意味着，
在Xcode模拟器中，可能注意不到Watchdog Timer。
因此最好在真实的设备中进行测试，检验代码长期运行时可能出现的问题。
```

##### `ARC`与非`ARC`的混合模式

`Xcode`项目中可以使用`ARC`和非`ARC`的混合模式

* 如果你的项目使用的是非`ARC`模式，则为`ARC`模式的代码文件加入 `-fobjc-arc`标签
* 如果你的项目使用的是`ARC`模式，则为非`ARC`模式的代码文件加入 `-fno-objc-arc`标签

* 添加标签的方法

```
1. 打开：你的target -> Build Phases -> Compile Sources
2. 双击对应的 *.m 文件
3. 在弹出窗口中输入上面提到的标签 -fobjc-arc / -fno-objc-arc
```

##### `fontSize`（字体大小）

![fontSize](iOS工具手册/fontSize.png)