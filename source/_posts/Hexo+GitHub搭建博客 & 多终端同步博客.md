---
title: Hexo+GitHub搭建博客 & 多终端同步博客
---
Hexo+GitHub搭建博客 & 多终端同步博客 for Mac

## Hexo+GitHub搭建博客

### 环境准备

* Git       [下载](https://git-scm.com)
* Node.js   [下载](https://nodejs.org/en/)
* GitHub账号 [注册](https://github.com)
* Markdown编辑器 [MacDown](https://macdown.uranusjr.com)

Tips:

```
在终端下查看Git、Node.js的安装情况
node -v
git --version
如已安装对应程序，会输出程序版本信息
```

### 安装Hexo

环境准备好后。终端输入如下命令安装Hexo脚手架：

```
sudo npm install -g hexo-cli
```

* 初始化
  * 终端cd到指定的目录，执行如下命令（hexoblog是自定义文件夹名称，是初始化时将要建立的文件夹）：
  
  ```
  hexo init hexoblog
  ```
  
  * cd 到hexoblog文件夹下，执行如下命令安装npm：
  
  ```
  npm install
  ```
  
  * 检验初始化是否成功，执行如下命令，开启本地Hexo服务器（如成功终端输出有http://localhost:4000的信息，浏览器打开http://localhost:4000的网址，能看到Hexo的页面）：

  ```
  hexo s
  ```
  
  Tips:
  
  ```
  如终端输入 hexo s 没有成功，
  可输入命令 hexo s -p 5000 改下端口试试看。
  ```
  
### 关联GitHub

###### 创建仓库

* 登录 [GitHub](https://github.com) 账号，创建名称格式为`xxx.github.io`的仓库，`xxx`为GitHub账号用户名。

###### 开启 GitHub Pages

* 在刚创建的仓库下，点击`setting`，进入`setting`界面，拖动到`GitHub Pages`，在`Source`处选择`master`分支并保存，如已是`master`分支不做此操作。

 Tips:
 
 ```
 可在 Custom domain 设置自己的域名，访问博客时直接输入此处设置的域名即可，不设置也不影响博客的访问，不过是浏览器输入地址时麻烦些，输入创建仓库时的仓库名 xxx.github.io。
 ```
 
 [域名注册](https://wanwang.aliyun.com/?spm=5176.8142029.735711.62.3d2c6d3ex1u7aA)
 
### 修改全局配置文件

终端cd到初始化时创建的hexoblog文件夹，找到 _config.yml 可通过`vim _config.yml`编辑 _config.yml 的deploy部分，编辑完后是下面的结果（clone地址的方式不同，结果会不一样）：

* Clone with SSH

```
deploy:
  type: git
  repository: git@github.com:xxx/xxx.github.io.git
  branch: master
```

* Clone with HTTPS

```
deploy:
  type: git
  repository: https://github.com/xxx/xxx.github.io.git
  branch: master
```

Tips:

```
xxx 是 GitHub 账号的用户名
修改配置文件 _config.yml 时，注意所有的冒号 : 后面都要加一个空格
```

### 上传博客到GitHub

修改完配置文件后，执行如下命令生成静态页面：

```
hexo g
```

如出错，尝试执行如下命令：

```
npm install hexo --save
```

如执行`hexo g`命令无报错，执行如下命令开始上传：

```
hexo d
```

如出错，执行如下命令来安装`hexo-deployer-git`：

```
npm insatll hexo-deployer-git --save
```

安装完成之后，再次执行`hexo g`和`hexo d`命令上传资源到GitHub。上传完成后可在浏览器输入`xxx.github.io`浏览已

