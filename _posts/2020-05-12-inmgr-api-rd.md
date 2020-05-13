---
layout: post
title: 接口领域分类
category: inmgr
tags: inmgr
bg: inmgr.png
---

接口平台接口领域设计相关文档

## 需求

### 引言

随着接口平台中接口的不断增加，接口平台暴露出一些问题：1）维护人员不知道系统中有哪些可以提供某某功能的接口。为实现接口平台的可持续发展，简化接口管理维护成本，故此迫切需要一个能快速定位接口分类，找到接口的这么一个功能。

### 需求概述

首先，需要有一个页面展示接口的领域分类，点击具体分类能快速查看该分类下的所有接口。其次，能够提供一个从其他不同维度探索接口的界面，维度有：时间、价格、关键字、来源等等...

### 系统功能需求

| 功能             | 描述                           | 备注 |
| ---------------- | ------------------------------ | ---- |
| 领域分类管理     | 维护一个接口平台特有的领域分类 |      |
| 接口领域分类     | 创建接口时需选择领域分类       |      |
| 搜索指定分类接口 | 一个能从不同维度探索接口的页面 |      |
|                  |                                |      |
|                  |                                |      |
|                  |                                |      |

#### 接口

1. 领域分类的crud
2. 根据领域探索接口
3. 其他



#### 数据字典

1. 领域分类表 api_class

   | 字段        | 含义     | 类型    | 长度 | 主键 | 默认值 |
   | ----------- | -------- | ------- | ---- | ---- | ------ |
   | id          | ID自增   | int     | 11   | 是   |        |
   | name        | 分类名称 | varchar | 64   |      | Null   |
   | description | 描述     | varchar | 128  |      | Null   |
   |             |          |         |      |      |        |

2. 接口信息表 新增字段

   | 字段     | 含义           | 类型 | 长度 | 主键 | 默认值 |
   | -------- | -------------- | ---- | ---- | ---- | ------ |
   | class_id | 接口所属领域ID | int  | 11   |      |        |
   |          |                |      |      |      |        |

   

## 详细设计

### 功能页面

#### 页面1-API领域分类主界面

| 功能点      | 位置         | 说明                                                         |
| ----------- | ------------ | ------------------------------------------------------------ |
| API探索     | top          | 主页面提供API探索功能，根据输入关键字展示接口                |
| 领域分类    | middle-left  | 展示常用的领域分类如：金融商业、生活服务、交通地理、吃喝玩乐... |
| 轮播展示区  | middle-right | 展示一些解决方案、服务特色等图片信息                         |
| 常用热门API | bottom       | 卡片方式展示热门API包括：名称、简介、价格、调用次数等指标    |

![1](/assets/inmgr/1.png)

#### 页面2-API市场

| 功能点                 | 位置   | 说明                                                         |
| ---------------------- | ------ | ------------------------------------------------------------ |
| 探索框                 | top    | 支持通过名称、分类等关键字探索API                            |
| 功能分类               | middle | 展示所有分类作为筛选条件                                     |
| 其他维度分类及探索条件 | middle | 展示其他筛选条件如：收费类型、API来源、付费方式、排序方式... |
| API列表                | bottom | 卡片方式展示根据筛选条件查询到的API                          |

![2](/assets/inmgr/2.png)

#### 页面3-API详情

这个页面类似当前接口平台系统中的接口文档页面。

| 功能点          | 位置   | 说明                                                |
| --------------- | ------ | --------------------------------------------------- |
| API概要         | top    | 展示API的名称、简介、来源、价格、购买链接等         |
| 其他信息tab页面 | middle |                                                     |
| Tab1            |        | 产品说明                                            |
| Tab2            |        | API文档（请求方式、参数、返回值、错误码表等的说明） |
| Tab3            |        | 代码示例                                            |
| Tab4            |        | 错误参照码表                                        |
| Tab5            |        | 其他相关                                            |
| 相关API         | bottom | 与此页面的API相关的API展示。                        |

![3](/assets/inmgr/3.png)





## 参考

### showapi

URL：[showapi](https://www.showapi.com/api/apiList)

**首页**

![1](/assets/inmgr/showapi-1.png)

**接口详情**

![2](/assets/inmgr/showapi-2.png)



### juhe

URL: [juhe](https://www.juhe.cn)

**首页**

![1](/assets/inmgr/juhe-1.png)

**分类页面**

![2](/assets/inmgr/juhe-2.png)

**接口详情**

![3](/assets/inmgr/juhe-3.png)



### avatardata

URL: [avatardata](https://www.avatardata.cn)

**首页**

![1](/assets/inmgr/avatardata-1.png)

**分类**

![2](/assets/inmgr/avatardata-2.png)

**接口详情**

![3](/assets/inmgr/avatardata-3.png)



### binstd

URL: [binstd](https://www.binstd.com)

**首页**

![0](/assets/inmgr/binstd-0.png)

**分类**

自营

![1](/assets/inmgr/binstd-1.png)

商城

![2](/assets/inmgr/binstd-2.png)

**接口详情**

![3](/assets/inmgr/binstd-3.png)



### yongyou

URL: [yongyou](https://api.yonyoucloud.com/#/home)

**首页**

![1](/assets/inmgr/yongyou-1.png)

**分类**

![2](/assets/inmgr/yongyou-2.png)

**接口详情**

![3](/assets/inmgr/yongyou-3.png)

### aliyun

URL: [aliyun](https://market.aliyun.com/data)

**首页**

![1](/assets/inmgr/aliyun-1.png)

**分类**

![2](/assets/inmgr/aliyun-2.png)

**接口详情**

![3](/assets/inmgr/aliyun-3.png)



### baidu

URL: [baidu](https://cloud.baidu.com/market/home)

**首页**

![1](/assets/inmgr/baidu-1.png)

**分类**

![2](/assets/inmgr/baidu-2.png)

**接口详情**

![3](/assets/inmgr/baidu-3.png)

![4](/assets/inmgr/baidu-4.png)

![5](/assets/inmgr/baidu-5.png)

### yesapi

URL: [yesapi](http://api.yesapi.cn/docs.html)

**首页**

![1](/assets/inmgr/yesapi-1.png)

**分类**

![2](/assets/inmgr/yesapi-2.png)

**接口详情**

![3](/assets/inmgr/yesapi-3.png)

![4](/assets/inmgr/yesapi-4.png)



### mxnzp

URL: [mxnzp](https://www.mxnzp.com)

**主页/分类**

![1](/assets/inmgr/mxnzp-1.png)

**接口详情**

![2](/assets/inmgr/mxnzp-2.png)



### jisuapi

URL: [jisuapi](https://www.jisuapi.com)

**首页**

![1](/assets/inmgr/jisuapi-1.png)

**分类**

![2](/assets/inmgr/jisuapi-2.png)

**接口详情**

![3](/assets/inmgr/jisuapi-3.png)

![3](/assets/inmgr/jisuapi-4.png)



### haoservice

URL: [haoservice](http://www.haoservice.com)

**首页**

![1](/assets/inmgr/haoservice-1.png)

**分类**

![2](/assets/inmgr/haoservice-2.png)

**接口详情**

![3](/assets/inmgr/haoservice-3.png)



### jdwx

URL：[京东万象](https://wx.jdcloud.com)

**首页**

![0](/assets/inmgr/jdwx-0.png)

**分类**

![1](/assets/inmgr/jdwx-1.png)

**接口详情**

![2](/assets/inmgr/jdwx-2.png)

![2](/assets/inmgr/jdwx-3.png)

![2](/assets/inmgr/jdwx-4.png)

