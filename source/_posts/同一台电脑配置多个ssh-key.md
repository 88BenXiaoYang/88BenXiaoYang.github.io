---
title: 同一台电脑配置多个ssh-key
date: 2019-01-02
---
当在同一台电脑上，对公司和自己的git项目进行操作时，会涉及到同一台电脑配置多个ssh-key。

## 生成和添加ssh-key

针对所要操作的[github](https://github.com)账号，生成对应的ssh-key。

#### 生成`ssh-key`

`ssh-keygen -t rsa -C ""`（""中的内容可随意取，任意字符串都行，没有特殊限制如必须为邮箱等这样的）。通常将生成的`ssh-key`放在`~/.ssh`文件夹下，遵循系统规则方便管理。

生成`ssh-key`可参考[Generating a new SSH key](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)

#### 添加`ssh-key`到`git`账号上

`cd .ssh` cd 到`ssh-key`所在目录

`cat xxx.pub` 查看公钥内容

将公钥内容添加到`github`账号上。可参考[Adding a new SSH key to your GitHub account](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/)

## 电脑上配置`host`

* `cd ~/.ssh`，cd 到 `~/.ssh`，在`config`中配置`host`信息。如没有`config`文件，用`vim config`创建并打开`config`文件
* 编辑`config`添加如下信息

```
Host test.github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa_test2
    User git

Host test3.github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa_test3
    User git
```

多个`ssh-key`配置，继续往下添加即可，注意保持队形～

信息格式模版

```
Host xxx.yyy.zzz
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa_xxx
    User git
```

`Host`后的`xxx.yyy.zzz`可随意取，最好方便标识，项目中`.git`文件夹下的`config`会用到这个`xxx.yyy.zzz`对`url`进行修改

#### 配置测试

`~/.ssh`下对`config`配置完成后，可通过`ssh -T test.github.com`（此处为`Host`后的名称）测试配置是否成功。
如成功，会打印出：

```
Hi github账号名称! You've successfully authenticated, but GitHub does not provide shell access.
```

如没打印上面信息，则没成功，应该是没把新生成的`ssh-key`的私钥添加到`ssh`的代理。使用`ssh-add ~/.ssh/ssh-key私钥`，将`ssh-key`的私钥添加到`ssh`的代理里面，重新通过`ssh -T test.github.com`测试配置是否成功（通常这样处理后会成功，其它情况遇到再解决～）

#### 修改项目的`git`配置

通过`ssh -T test.github.com`测试配置成功后，修改项目中的`git`配置。项目中的`.git`文件夹下`config`中的`url`一般情况如下：

```
[remote "origin"]
    url = git@github.com:github账号名/仓库名.git
```

修改后的结果如下：

```
[remote "origin"]
    url = git@xxx.yyy.zzz:github账号名/仓库名.git
```

经过上述操作流程后，能正常执行`git pull`、`git push`即同一台电脑配置多个`ssh-key`成功～