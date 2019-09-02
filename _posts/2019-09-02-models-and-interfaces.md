---
layout: post
title:  "模型和接口设计"
date:   2019-09-02 22:28:00 +0800
---

首先需要设计数据模型和接口，主要包括用户，认证，关注，Post几个模块。

## 模型

### User模型

User模型包含用户的认证信息，也可以称为Account。

| 字段         | 含义         | 类型                |
| ------------ | ------------ | ------------------- |
| id           | 用户ID       | Integer/String/UUID |
| username     | 用户名       | String              |
| email        | 邮箱         | String              |
| password     | 密码的hash值 | String              |
| time_created | 创建时间     | DateTime            |

### Profile模型

Profile包含用户的简介，User和Profile为一对一关系。

| 字段     | 含义       | 类型                             |
| -------- | ---------- | -------------------------------- |
| id       | ID         | Integer/String/UUID              |
| user     | 关联的User | ForeignKey(User)                 |
| nickname | 昵称       | String                           |
| gender   | 性别       | Enum(UNKNOW=0, MALE=1, FEMALE=2) |
| avatar   | 头像       | Image/URL                        |

### Authentication模型

Authentication包括用户的登录Token，用于接口认证。

| 字段         | 含义                                                       | 类型               |
| ------------ | ---------------------------------------------------------- | ------------------ |
| token        | Token，每个Token唯一标识一个用户，所以Unique，可以作为主键 | String，PrimaryKey |
| user         | 关联的User                                                 | ForeignKey(User)   |
| time_created | 创建时间，用于token失效验证                                | DateTime           |

### Following模型

Following包含用户之间的相互关注关系。

| 字段          | 含义           | 类型                |
| ------------- | -------------- | ------------------- |
| id            | ID             | Integer/String/UUID |
| from_user     | 发起关注的用户 | ForeignKey(User)    |
| to_user       | 被关注的用户   | ForeignKey(User)    |
| time_followed | 关注时间       | DateTime            |

### Post模型

| 字段         | 含义                             | 类型                |
| ------------ | -------------------------------- | ------------------- |
| id           | ID                               | Integer/String/UUID |
| parent       | 转发的Post，如果不是转发则为NULL | ForeignKey(Post)    |
| content      | Post的内容                       | String              |
| owner        | Post的作者                       | ForeignKey(User)    |
| time_created | 创建时间                         | DateTime            |

### Comment模型

Comment为Post的评论。

| 字段         | 含义                                   | 类型                |
| ------------ | -------------------------------------- | ------------------- |
| id           | ID                                     | Integer/String/UUID |
| post         | 所评论的Post                           | ForeignKey(Post)    |
| parent       | 回复的评论，如果不是回复评论，则为NULL | ForeignKey(Comment) |
| content      | 评论的内容                             | String              |
| owner        | 评论者                                 | ForeignKey(User)    |
| time_created | 评论时间                               | DateTime            |

### Like模型

Like为Post的点赞。

| 字段       | 含义         | 类型                |
| ---------- | ------------ | ------------------- |
| id         | ID           | Integer/String/UUID |
| post       | 所点赞的Post | ForeignKey(Post)    |
| user       | 点赞者       | ForeignKey(User)    |
| time_liked | 点赞时间     | DateTime            |

*post，user的组合唯一*

## 接口

| Endpoint                   | 方法   | 说明                     |
| -------------------------- | ------ | ------------------------ |
| /users/                    | POST   | 创建用户                 |
| /users/                    | GET    | 获取用户列表             |
| /users/{id}/               | GET    | 获取指定ID的用户的信息   |
| /users/{id}/followings/    | GET    | 获取指定ID用户的关注列表 |
| /users/{id}/followers/     | GET    | 获取指定ID用户的粉丝列表 |
| /users/{id}/posts/         | GET    | 获取指定ID用户的post列表 |
| /user/                     | GET    | 获取当前登录用户的信息   |
| /user/                     | PATCH  | 修改当前登录用户的信息   |
| /authentications/          | POST   | 登录/获取Token           |
| /authentications/          | DELETE | 注销/删除Token           |
| /followings/               | POST   | 关注用户                 |
| /followings/               | DELETE | 取消关注用户             |
| /posts/                    | POST   | 创建post                 |
| /posts/                    | GET    | 获取post列表             |
| /posts/{id}/               | DELETE | 删除post，需要是作者     |
| /posts/{id}/comments/      | POST   | 评论post                 |
| /posts/{id}/comments/      | GET    | 获取指定ID的post的评论   |
| /posts/{id}/comments/{id}/ | DELETE | 删除评论                 |
| /posts/{id}/like/          | POST   | 点赞指定ID的post         |
| /posts/{id}/like/          | DELETE | 取消指定ID的post         |
| /posts/feed/               | GET    | 获取关注用户发的post     |

限于篇幅，接口的调用参数和返回参数没有在表中给出，但基本与模型的字段一致，有以下几点需要注意：

* user/users接口同时包含Profile的字段

* user/users接口同时包含关注数、粉丝数、post数字段

* 没有设计在修改username/password/email时的邮件验证，没有忘记密码接口，因此email字段其实可有可无

* ``POST /authentications/`` 传入用户名和密码，返回token