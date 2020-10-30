---
layout: post
title: 接口领域分类
category: inmgr
tags: inmgr
bg: inmgr.png
---

# 2020-05-12-inmgr-api-rd

接口平台接口领域设计相关文档

### 需求

#### 引言

随着接口平台中接口的不断增加，接口平台暴露出一些问题：1）维护人员不知道系统中有哪些可以提供某某功能的接口。为实现接口平台的可持续发展，简化接口管理维护成本，故此迫切需要一个能快速定位接口分类，找到接口的这么一个功能。

#### 需求概述

首先，需要有一个领域分类管理的功能，能够管理系统中的所有分类包括（增删改查）。其次，要有一个“API市场”的功能页面，具体展示领域分类和一些热门的接口。点击具体分类能快速查看该分类下的所有接口；另外能够提供一个从其他不同维度探索接口的界面，维度有：时间、价格、关键字、来源等等...

#### 系统功能需求

| 功能 | 描述 | 备注 |
| :--- | :--- | :--- |
| 领域分类管理 | 维护一个接口平台特有的领域分类 |  |
| 接口领域分类设置 | 创建接口时需选择领域分类，接口创建/编辑成功后在api\_category\_relation中维护分类和接口的对应关系 |  |
| 接口市场 | 从不同维度探索接口的页面 |  |
|  |  |  |

**接口设计**

1. 领域分类的crud

   维护系统中的领域分类，包括增删查改

   > 1. 增：新建领域分类，确保领域名称唯一性。
   > 2. 删：删除领域分类，删除前校验该分类下是否存在接口，不存在则删除，否则提示信息。
   > 3. 查：1）页面分页查询。
   > 4. 改：跟新领域分类信息，校验唯一性。

2. 根据领域探索接口

   根据领域探索接口

   > 1. 查询系统所有领域分类或者热门分类，页面展示用
   > 2. 根据领域分类ID探索改领域的接口，页面点击领域分类链接，查询该领域的所有接口
   > 3. 展示

3. 其他

   其他接口

   > 1. 维护接口icon，增删查改，上传下载
   > 2. 实时替换

**数据字典**

1. 领域分类表 api\_category

   | 字段 | 含义 | 类型 | 长度 | 主键 | 默认值 |
   | :--- | :--- | :--- | :--- | :--- | :--- |
   | id | ID自增 | int | 11 | 是 |  |
   | name | 分类名称 | varchar | 64 |  | Null |
   | description | 描述 | varchar | 128 |  | Null |
   |  |  |  |  |  |  |

2. 接口领域关联表 api\_category\_relation

   | 字段 | 含义 | 类型 | 长度 | 主键 | 默认值 |
   | :--- | :--- | :--- | :--- | :--- | :--- |
   | id | ID自增 | int | 11 | 是 |  |
   | class\_id | 接口所属领域ID | int | 11 |  |  |
   | api\_id | 接口ID | int | 11 |  |  |

### 详细设计

#### 功能页面

**页面1-API领域分类主界面**

| 功能点 | 位置 | 说明 |
| :--- | :--- | :--- |
| API探索 | top | 主页面提供API探索功能，根据输入关键字展示接口 |
| 领域分类 | middle-left | 展示常用的领域分类如：金融商业、生活服务、交通地理、吃喝玩乐... |
| 轮播展示区 | middle-right | 展示一些解决方案、服务特色等图片信息 |
| 常用热门API | bottom | 卡片方式展示热门API包括：名称、简介、价格、调用次数等指标 |

![1](../.gitbook/assets/1%20%283%29.png)

**页面2-API市场**

| 功能点 | 位置 | 说明 |
| :--- | :--- | :--- |
| 探索框 | top | 支持通过名称、分类等关键字探索API |
| 功能分类 | middle | 展示所有分类作为筛选条件 |
| 其他维度分类及探索条件 | middle | 展示其他筛选条件如：收费类型、API来源、付费方式、排序方式... |
| API列表 | bottom | 卡片方式展示根据筛选条件查询到的API |

## ![2](../.gitbook/assets/2%20%283%29.png)

**页面3-API详情**

这个页面类似当前接口平台系统中的接口文档页面。

| 功能点 | 位置 | 说明 |
| :--- | :--- | :--- |
| API概要 | top | 展示API的名称、简介、来源、价格、购买链接等 |
| 其他信息tab页面 | middle |  |
| Tab1 |  | 产品说明 |
| Tab2 |  | API文档（请求方式、参数、返回值、错误码表等的说明） |
| Tab3 |  | 代码示例 |
| Tab4 |  | 错误参照码表 |
| Tab5 |  | 其他相关 |
| 相关API | bottom | 与此页面的API相关的API展示。 |

![3](../.gitbook/assets/3%20%283%29.png)

### 参考

#### showapi

URL：[showapi](https://www.showapi.com/api/apiList)

**首页**

![1](../.gitbook/assets/showapi-1%20%281%29.png)

**接口详情**

![2](../.gitbook/assets/showapi-2%20%281%29.png)

#### juhe

URL: [juhe](https://www.juhe.cn)

**首页**

![1](../.gitbook/assets/juhe-1%20%281%29.png)

**分类页面**

![2](../.gitbook/assets/juhe-2%20%281%29.png)

**接口详情**

![3](../.gitbook/assets/juhe-3%20%281%29.png)

#### avatardata

URL: [avatardata](https://www.avatardata.cn)

**首页**

![1](../.gitbook/assets/avatardata-1%20%281%29.png)

**分类**

![2](../.gitbook/assets/avatardata-2%20%281%29.png)

**接口详情**

![3](../.gitbook/assets/avatardata-3%20%281%29.png)

#### binstd

URL: [binstd](https://www.binstd.com)

**首页**

![0](../.gitbook/assets/binstd-0%20%281%29.png)

**分类**

自营

![1](../.gitbook/assets/binstd-1%20%281%29.png)

商城

![2](../.gitbook/assets/binstd-2%20%281%29.png)

**接口详情**

![3](../.gitbook/assets/binstd-3%20%281%29.png)

#### yongyou

URL: [yongyou](https://api.yonyoucloud.com/#/home)

**首页**

![1](../.gitbook/assets/yongyou-1%20%281%29.png)

**分类**

![2](../.gitbook/assets/yongyou-2%20%281%29.png)

**接口详情**

![3](../.gitbook/assets/yongyou-3%20%281%29.png)

#### aliyun

URL: [aliyun](https://market.aliyun.com/data)

**首页**

![1](../.gitbook/assets/aliyun-1%20%281%29.png)

**分类**

![2](../.gitbook/assets/aliyun-2%20%281%29.png)

**接口详情**

![3](../.gitbook/assets/aliyun-3%20%281%29.png)

#### baidu

URL: [baidu](https://cloud.baidu.com/market/home)

**首页**

![1](../.gitbook/assets/baidu-1%20%281%29.png)

**分类**

![2](../.gitbook/assets/baidu-2%20%281%29.png)

**接口详情**

![3](../.gitbook/assets/baidu-3%20%281%29.png)

![4](../.gitbook/assets/baidu-4%20%281%29.png)

![5](../.gitbook/assets/baidu-5%20%281%29.png)

#### yesapi

URL: [yesapi](http://api.yesapi.cn/docs.html)

**首页**

![1](../.gitbook/assets/yesapi-1%20%281%29.png)

**分类**

![2](../.gitbook/assets/yesapi-2%20%281%29.png)

**接口详情**

![3](../.gitbook/assets/yesapi-3%20%281%29.png)

![4](../.gitbook/assets/yesapi-4%20%281%29.png)

#### mxnzp

URL: [mxnzp](https://www.mxnzp.com)

**主页/分类**

![1](../.gitbook/assets/mxnzp-1%20%281%29.png)

**接口详情**

![2](../.gitbook/assets/mxnzp-2%20%281%29.png)

#### jisuapi

URL: [jisuapi](https://www.jisuapi.com)

**首页**

![1](../.gitbook/assets/jisuapi-1%20%281%29.png)

**分类**

![2](../.gitbook/assets/jisuapi-2%20%281%29.png)

**接口详情**

![3](../.gitbook/assets/jisuapi-3%20%281%29.png)

![3](../.gitbook/assets/jisuapi-4%20%281%29.png)

#### haoservice

URL: [haoservice](http://www.haoservice.com)

**首页**

![1](../.gitbook/assets/haoservice-1%20%281%29.png)

**分类**

![2](../.gitbook/assets/haoservice-2%20%281%29.png)

**接口详情**

![3](../.gitbook/assets/haoservice-3%20%281%29.png)

#### jdwx

URL：[京东万象](https://wx.jdcloud.com)

**首页**

![0](../.gitbook/assets/jdwx-0%20%281%29.png)

**分类**

![1](../.gitbook/assets/jdwx-1%20%281%29.png)

**接口详情**

![2](../.gitbook/assets/jdwx-2%20%281%29.png)

![2](../.gitbook/assets/jdwx-3%20%281%29.png)

![2](../.gitbook/assets/jdwx-4%20%281%29.png)

