---
layout: post
title:  "MiniPost项目说明"
date:   2019-08-31 21:28:00 +0800
---

在学习后台开发的知识时，很多地方需要实践，因此建了这个项目希望能够尝试从单机服务器架构，到数据库读写分离，到微服务架构等一系列的过程。同时，对后台的性能测试，压力测试也做一些尝试。

业务功能参考微博/Twitter，包含其最简单的核心功能，项目取名MiniPost。由于功能简单，因此直接说明需求，不做开发迭代管理。

## 功能说明

首先，有用户系统，可以进行注册和登录，原型图如下：

![Login](/assets/minipost-login.png)

![Signup](/assets/minipost-signup.png)

登录以后可以发送Post，Post为一段文字。用户关注其他用户以后可以在首页看到关注用户所发的Post，首页原型图如下：

![Home](/assets/minipost-home.png)

用户可以查看自己或其他用户的个人信息，个人信息包括昵称和性别，通过标签切换可以查看用户的关注、粉丝和所发的Post，原型如下：

![User Profile - Post](/assets/minipost-user-post.png)

![User Profile - Follow](/assets/minipost-user-follow.png)

原型图中有不完整的地方，如注册时的性别填写，创建Post的按钮和对话框，以及修改个人信息页面等，后续会通过Vue做一个简单的前端进行测试。
