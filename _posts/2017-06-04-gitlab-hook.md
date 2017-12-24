---
layout: post
title:  "如何在gitlab上设置钩子"
date:   2017-06-04 20:29:54
categories: gitlab
tags: gitlab
analytics: true
---

* content
{:toc}

假如我有个项目A，我在项目A上做了修改，当我把相应的修改提交到gitlab上后，我希望它能自动部署或者跑一些自动化脚本，这个时候就需要用到了gitlab钩子。

钩子，简单地可以理解为我的某项操作提交到gitlab服务器后会触发另一项操作。

那怎么来设置这个钩子呢？

我们可以找到gitlab上我们的项目所在位置，一般为：/var/opt/gitlab/Git-data/repositories/your_name/your_project.git，其中your_name为你的名称，your_project为你的项目名称。

我们cd进入这个目录，在这个目录下新建一个文件夹叫custom_hooks

执行以下命令更改权限

```bash
chmod 777 custom_hooks
```

更改所属者和所属组

```bash
chown git:git custom_hooks
```

以上操作需要root权限，没有权限请sudo到root用户

进入custom_hooks,

```bash
cd custom_hooks
```

在该目录下，我们就可以新建我们的钩子了，钩子必须以特定名称命名，具体参考https://git-scm.com/docs/githooks，我们以post-receive为例，表示gitlab收到客户端推送的代码后要执行的操作。

新建文件post-receive

```bash
touch post-receive
```

同样需要修改权限和用户

```bash
chmod 777 post-receive
chown git:git post-receive
```

假设我们的部署目录位于/www下，则我们先在该目录下初始化我们的git项目：

```bash
cd /www
git clone "/var/opt/gitlab/git-data/repositories/your_name/your_project.git"
```

回到post-receive所在目录，我们使用post-receive文件编写我们的自动化脚本

```bash
#!/bin/bash
cd "/www/your_project"
unset GIT_DIR
git pull origin master
```

这里的脚本只是简单对部署目录进行更新，你可以随意编写自己的脚本。其中unset GIT_DIR这个命令必须要有，若没有，后面的git pull origin master不会执行。