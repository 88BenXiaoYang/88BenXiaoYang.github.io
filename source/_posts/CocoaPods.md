---
title: CocoaPods
date: 2019-03-04 15:35:13
tags: iOS
categories: iOS
---

##### 简介

`CocoaPods`是一个`Xcode`项目下管理类库依赖的工具。在使用`CocoaPods`之后，只需要将用到的第三方开源库的名字放到`Podfile`文件中，然后执行`pod install`。`CocoaPods`就会自动将`podfile`文件中的第三方开源库的源码下载下来，并且为工程设置好相应的系统依赖和编译参数。

`CocoaPods`的介绍可见[What is CocoaPods](https://guides.cocoapods.org/using/getting-started.html)

##### 安装

`CocoaPods`是用`Ruby`写的工具。因此安装`CocoaPods`之前要先安装`Ruby`。在`Ruby`环境下，通过`gem`来安装`CocoaPods`。

* 安装`Ruby`、`gem`

`Ruby`的安装方式有多种。具体安装方式见[安装 Ruby](http://www.ruby-lang.org/zh_cn/documentation/installation/)。

`MacOS`环境下，通常会使用包管理系统工具`Homebrew`，获取安装最新的`Ruby`，安装命令为：`brew install ruby`。`RubyGems`从`Ruby1.9`版起成为`Ruby`标准库的一部分，可见[RubyGems 介绍](http://www.runoob.com/ruby/ruby-rubygems.html)。因此安装最新版本的`Ruby`就有`gem`包管理工具。

可通过`brew -v`查看`Homebrew`的版本信息，如未安装`Homebrew`，参考[安装 Homebrew](https://brew.sh/index_zh-cn)。

如需手动安装`gem`，参考[安装 RubyGems](https://rubygems.org/pages/download)。

通过`gem help commands`查看`gem`的对应命令。如查看`gem`的安装路径用`gem environment gemdir`。

* 安装`CocoaPods`

使用淘宝的镜像安装Ruby的第三方库，修改gem的镜像：

```
gem sources --remove https://rubygems.org/
gem sources -a https://ruby.taobao.org/
```

为了验证`Ruby`镜像是并且仅是淘宝，可以用以下命令查看：`gem sources -l`

只有在终端中出现下面文字才表明修改`gem`的镜像命令是成功的：

```
*** CURRENT SOURCES ***
https://ruby.taobao.org/
```

修改`gem`镜像成功后，终端中执行命令`sudo gem install cocoapods`安装`CocoaPods`。安装完成后，执行命令`pod setup`初始化`CocoaPods`的环境。

Tips:

官宣安装介绍，可见[Getting Started](https://guides.cocoapods.org/using/getting-started.html#getting-started)。

##### 使用

* 创建`Xcode`工程并`cd`到工程路径
* 使用命令`pod init`，此时会在当前路径下生成`Podfile`文件
* 编辑`Podfile`文件，添加需要使用的第三方库的名字，如：

```
platform :ios, '9.0'
pod 'Masonry'
pod 'SDWebImage'
```

往`Podfile`添加第三方库的格式为：`pod '第三方库名称', '版本号'`

版本号标识的区别：

```
'> 0.1' Any version higher than 0.1
'>= 0.1' Version 0.1 and any higher version
'< 0.1' Any version lower than 0.1
'<= 0.1' Version 0.1 and any lower version

'~> 0.1.2' Version 0.1.2 and the versions up to 0.2, not including 0.2 and higher
'~> 0.1' Version 0.1 and the versions up to 1.0, not including 1.0 and higher
'~> 0' Version 0 and higher, this is basically the same as not having it.

有版本号，但不加任何操作符修饰，则为下载安装指定版本号的库。
无版本号，则为下载安装最新版本号的库。
```

`Podfile`的编辑规范，可见[The Podfile](https://guides.cocoapods.org/using/the-podfile.html)

* 安装工程依赖的第三方库

执行命令`pod install`	，出现`pods installed`提示，则表示所需的第三方库安装成功。

```
如是初始配置工程使用 CocoaPods，则在执行 pod install 成功后，项目文件夹下会新增 xxx.xcworkspace、Podfile.lock、Pods
```

* 打开`.xcworkspace`文件，在需要使用第三方库的类中导入第三方库文件，如`#import <Masonry.h>`即可。