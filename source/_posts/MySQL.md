---
title: MySQL
date: 2019-02-06 16:38:40
tags: 数据库
categories: 数据库
---

##### 常用操作

* 进入数据库（登录mysql）：`mysql -u root -p`

* 修改密码
```
set password for ‘root’@‘localhost’ = password(‘新密码’);
```
  * 修改密码归类（四种在MySQL中修改root密码的方法）
  
  ```
  方法1: 用SET PASSWORD命令
          mysql -u root
          mysql> SET PASSWORD FOR 'root'@'localhost' = PASSWORD('newpass');
  ```
  
  ```
  方法2: 用mysqladmin
          mysqladmin -u root password "newpass"
          如果root已经设置过密码，采用如下方法
          mysqladmin -u root password oldpass "newpass"
  ```
  
  ```
  方法3: 用UPDATE直接编辑user表
          mysql -u root
          mysql> use mysql;
          mysql> UPDATE user SET Password = PASSWORD('newpass') WHERE user = 'root';
          mysql> FLUSH PRIVILEGES;
  ```
  
  ```
  方法4: 如果你忘记了或者不知道初始密码
         step1:
         苹果->系统偏好设置->最下边点mysql 在弹出页面中 关闭mysql服务（点击stop mysql server）
         
         step2:
         进入终端输入：cd /usr/local/mysql/bin/
         回车后 登录管理员权限 sudo su
         回车后输入以下命令来禁止mysql验证功能 ./mysqld_safe --skip-grant-tables &
         回车后mysql会自动重启（偏好设置中mysql的状态会变成running）
         
         step3:
         输入命令 ./mysql
         回车后，输入命令 FLUSH PRIVILEGES;
         回车后，输入命令 SET PASSWORD FOR 'root'@'localhost' = PASSWORD('你的新密码’);
  ```
  
  至此，密码修改完成。
  
* 如何查看Mysql中有哪些数据库和表

```
想要知道自己的Mysql中有哪些数据库和表：
show databases;//查看有哪些数据库
use databaseName;//指定使用哪个数据库
show tables;//查看指定数据库下都有哪些表
```

* 使用前要指定具体数据库名称: `use 数据库名称;`

* 查看当前所使用数据库的名称（三种方法）

```
a、select database();
b、show tables;
c、status;
```

* 查看数据库端口号: `show global variables like ‘port’;`

**注: 项目中操作数据库时，需要先查询判断目标数据库是否存在，若存在再进行后续查询等操作。因在打开数据库时，若数据库不存在，系统会新建一个同名数据库，因此时新建的数据库内容为空，执行SQL API时会不成功。**

* MySQL建表时的规范

```
1、表必备三个字段：id、gmt_create、gmt_modified。
其中id必为主键,类型为unsigned bigint、单表时自增、步长为1。
gmt_create, gmt_modified 的类型均为 date_time 类型。

2、表的命名最好是加上“业务名称_表的作用”。

3、单表行数超过 500 万行或者单表容量超过 2GB,才推荐进行分库分表。
如果预计三年后的数据量根本达不到这个级别,请不要在创建表时就分库分表。
```

* ORM规约

```
1、xml 配置中参数注意使用:#{},#param# 不要使用${} 此种方式容易出现 SQL 注入。
```

* 开启数据库服务

```
使用：mysql -u root -p进入数据库时，若出现zsh: command not found: mysql，则是数据库服务未开启
使用：/usr/local/bin -> sudo ln -fs /usr/local/mysql/bin/mysql mysql 输入电脑密码（ln -s或ln -fs是做链接操作）
使用：mysql --version，若输出版本信息，则数据库服务成功开启
使用：mysql -u root -p，输入数据库密码，进入数据库
```


  