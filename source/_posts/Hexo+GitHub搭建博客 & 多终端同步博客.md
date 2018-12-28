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

Hexo介绍及常用操作阅读[Hexo文档](https://hexo.io/zh-cn/docs/)

环境准备好后。终端输入如下命令安装Hexo脚手架（安装完成后可通过`hexo -v`查看Hexo的版本信息）：

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
 可在 Custom domain 设置自己的域名，
 访问博客时直接输入此处设置的域名即可。
 不设置也不影响博客的访问，不过是浏览器输入地址时麻烦些，
 输入创建仓库时的仓库名 xxx.github.io。
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
修改配置文件 _config.yml 时，
注意所有的冒号 : 后面都要加一个空格
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

安装完成之后，再次执行`hexo g`和`hexo d`命令上传资源到GitHub。上传完成后可在浏览器输入`xxx.github.io`浏览已搭建好的博客。

## 多终端同步博客

博客搭建完成，想在不同终端进行博客的发布更新，需进行如下操作。

思路：将博文内容相关文件放在GitHub项目的master中，将Hexo写博客用的相关配置文件放在GitHub项目的hexo分支上，多终端的同步只需要对hexo分支进行操作。

上面搭建博客操作`hexo d`命令后，会把博文内容相关的文件上传到GitHub上的xxx.github.io项目的master分支上。

接下来：

* cd 到hexoblog文件夹下，`git init`初始化本地仓库
* 依次添加必要的文件

```
git add _config.yml
git add package.json
git add scaffolds
git add source
git add themes
git add .gitignore
```

不用`git add .`进行添加，是因为用`git add .`会把`npm install`产生的 `node_modules` 给一起添加了，`node_modules` 不必添加，在另一终端操作博客时，执行`npm install`会产生`node_modules`。

* 提交操作记录 `git commit -m "添加必要文件"`
* 新建分支 `git branch hexo`
* 切换到hexo分支上 `git checkout hexo`
* 将本地与GitHub上的项目关联起来
`git remote add origin git@github.com:xxx/xxx.github.io.git`
* 将本地的hexo分支push到GitHub项目的hexo分支上
`git push origin hexo`

经过上述操作后GitHub项目上会增加hexo分支，hexo分支下是之前add的文件 `_config.yml、package.json、scaffolds 、ource、themes、.gitignore`。这个hexo分支就是用于多终端同步操作博客的关键部分。

### 模拟另一终端操作博客

新建一个临时文件夹tmpblog

* `cd tmpblog`
* `git init`
* 将GitHub项目上的hexo分支clone到本地
 `git clone -b hexo github上博客的项目地址`
* cd 到刚clone的文件夹
* `npm install`
* 新建博客文章`hexo new post "博客文章名"`
* `git add source` git add hexo分支中有改动的文件
* `git commit -m "add new blog file"`
* 更新修改到GitHub上的hexo分支`git push origin hexo`
* `hexo d -g`构建上传文章到GitHub的master分支上

通过上述操作后，在浏览器浏览xxx.github.io即可看到新创建的博客文章。

Tips：

```
在另一台终端进行如上操作前，先确认终端是否已安装了git、Hexo、Node.js程序
可通过git --version、hexo -v、node -v进行查看
如输出对应的版本信息，说明已有发布、更新博客的环境，继续上述操作
如环境没准备好，看上文 ‘环境准备’，操作到Hexo脚手架安装完成即可
```

```
上述 git add 时，只需git add hexo分支中有改动的文件
```

###### 可能会遇到的问题

执行`hexo d -g`后，浏览器不能正常显示博客内容，终端上会提示`WARN No layout: index.html`，解决方案可参考[博客](http://www.cnblogs.com/LandWind/articles/8952362.html)

* `git clone https://github.com/iissnan/hexo-theme-next themes/next`
* `git add themes`
* `git commit -m "clone themes"`
* `git pull origin hexo`
* `git push origin hexo`
* `hexo clean`
* `hexo d -g`
