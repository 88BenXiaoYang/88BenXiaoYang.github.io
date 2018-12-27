---
title: Hexo+GitHub搭建博客 & 多终端同步博客
---
Hexo+GitHub搭建博客 & 多终端同步博客 for Mac

## Hexo+GitHub搭建博客

### 环境准备

* Git       [下载地址](https://git-scm.com)
* Node.js   [下载地址](https://nodejs.org/en/)
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

###### [创建仓库](https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=1&rsv_idx=1&tn=baidu&wd=github%E5%88%9B%E5%BB%BA%E4%BB%93%E5%BA%93&oq=github%25E5%2588%259B%25E5%25BB%25BA%25E4%25BB%2593%25E5%25BA%2593&rsv_pq=f1e936160003d7c0&rsv_t=c77enS794y8tmRMRyIv8%2BounbNjyjFL0PactJQUxkRycSrPchE%2Ftg66SsM4&rqlang=cn&rsv_enter=1&inputT=7&rsv_sug3=19&rsv_sug1=14&rsv_sug7=100&rsv_sug2=0&rsv_sug4=574&rsv_sug=1)
