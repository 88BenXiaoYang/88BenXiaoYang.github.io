---
title: Hexo+GitHub博客常用功能
date: 2019-01-01
tags:
---

* 博文插入图片

```
1.修改配置文件 _config.yml 里的 post_asset_folder: 这个选项设置为true

2.在博客项目目录下执行：
npm install hexo-asset-image --save
下载安装上传本地图片的插件，这个插件来自dalao：dalao的git

3.安装完插件后，执行hexo n "xxxx"来生成md博文时，
/source/_posts文件夹内除了xxxx.md文件还有一个同名的文件夹
(该同名文件夹也可以手动创建)

4.在xxxx.md中想引入图片时，先把图片复制到xxxx这个文件夹中，
然后只需要在xxxx.md中按照markdown的格式引入图片：
markdown引入图片的格式：
![替代文字](xxxx/图片名.jpg)

注意： xxxx是这个md文件的名字，也是同名文件夹的名字。
只需要有文件夹名字即可，不需要有什么绝对路径。
想引入的图片只需要放入xxxx这个文件夹内即可。
```

##### 常见问题及解决方法

* 构建博客的时候出现`Template render error: (unknown path)`

```
原因：这个错误场景是 Hexo 转义的时候发生的错误，文章中可能出现了{{}}，{%%}等字符代码。

解决方法：将出现的特殊字符代码，用 ` 进行注释即可。
（对指定字符串进行注释没解决问题，现阶段是对包含 {{}} 的文本段，进行整段的注释）
```