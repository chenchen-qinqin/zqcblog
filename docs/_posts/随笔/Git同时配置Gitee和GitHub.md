---
title: Git同时配置Gitee和GitHub
date: 2023-11-05 16:28:24
permalink: /pages/c492ee/
sidebar: auto
categories:
  - 随笔
tags:
  - 
author: 
  name: zqingchen
  link: https://github.com/chenchen-qinqin
---
# Git同时配置Gitee和GitHub

Git同时配置Gitee和GitHub，可实现一台机器将一个项目同时提交到GitHub和Gitee（码云）上。配置分两种情况，一种是Gitee和GitHub使用不同的邮箱账户，另一种是使用相同的邮箱账户。

## 使用不同的邮箱账户

我们先要查看Git中是否配置了全局的用户名称和邮箱：

```bash
git config --global -l
```

![image-20231024093816838](https://qny.zqcblog.us.kg/2023/11/image-20231024093816838.png)

如果有需要取消全局的用户名和邮箱，没有无需执行下面的语句：

```bash
git config --global --unset user.name "XXX"
git config --global --unset user.email "xxx@xx.com"
```

下面查询结果为空，表示取消成功

![image-20231024092112522](https://qny.zqcblog.us.kg/2023/11/image-20231024092112522.png)

接下来先进入~/.ssh目录，然后分别生成Gitee和GitHub的SSH秘钥，两行代码依次执行：

```bash
cd ~/.ssh
ssh-keygen -t rsa -C "Gitee邮箱账号"
ssh-keygen -t rsa -C "GitHub邮箱账号"
```

下面以执行第一条代码为例，回车后，输入id_rsa_gitee回车，然后再次回车

![image-20231024093333542](https://qny.zqcblog.us.kg/2023/11/image-20231024093333542.png)

同样的操作，我们执行第二条语句。**注意：**要使用GitHub的邮箱，回车输入id_rsa_github。

接下来，可以使用pwd命令得到秘钥所在的路径：

![image-20231024143908471](https://qny.zqcblog.us.kg/2023/11/image-20231024143908471.png)

![image-20231024144023952](https://qny.zqcblog.us.kg/2023/11/image-20231024144023952.png)

把id_rsa_gitee.pub和id_rsa_github.pub记事本打开，然后分别添加到Gitee和GitHub公钥中

Gitee添加**SSH公钥**：

![image-20231024144509593](https://qny.zqcblog.us.kg/2023/11/image-20231024144509593.png)

GitHub添加**SSH公钥**：

![image-20231024144847839](https://qny.zqcblog.us.kg/2023/11/image-20231024144847839.png)

**创建配置文件：**

在 .ssh 文件夹中创建 config 文件，添加以下内容以区分两个 ssh key：

```bash
# gitee
Host gitee.com
HostName gitee.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_gitee

# github
Host github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_github
```

**测试连接是否正常**

在命令行分别输入：

```bash
ssh -T git@gitee.com
ssh -T git@github.com
```

![image-20231024145647858](https://qny.zqcblog.us.kg/2023/11/image-20231024145647858.png)

## 使用相同的邮箱账户

Gitee和GitHub使用相同的邮箱，这时是否配置全局的邮箱都可以。并只需生成一个邮箱的id_rsa公钥，然后分别添加到Gitee和GitHub公钥中。

## 上传代码

![image-20231025090449496](https://qny.zqcblog.us.kg/2023/11/image-20231025090449496.png)

设置局部的`user.email`和`user.name`

方法一：控制台输入

```bash
git config user.email "你的邮箱"
git config user.name "你的用户名"
```

方法二：在.git文件夹下config文件中添加[user]内容

![image-20231104235930442](https://qny.zqcblog.us.kg/2023/11/image-20231104235930442.png)

在文件夹中，右键，点击Git Bash Here，依次执行下面的语句：

```bash
git add .
git commit -m "填入提交的描述"
git push
```
