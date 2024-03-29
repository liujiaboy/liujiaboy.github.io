---
title: iOS网络请求封装
date: 2022-12-29 14:49:59
hidden: true
tags:
    - 架构
categories:
    - 架构 
---

网络层能提供的大致功能大家其实都比较清楚，无非是get、post等方式的API请求，还有文件上传下载，更大型的app会提供各种参数控制，如缓存、重试机制，安全策略等。网络层的好坏更多体现在设计上，如何给app提供给友好的调用方式，在这个基础上再提供更多的小功能小接口。

 

**一、离散型API和集约型API**

1.从上层来说其实所有的请求都是集约型API请求，因为都需要走AFNetworking，把参数扔过去，把返回结果拿到。对于一些小型APP来说，只有简单的网络请求，甚至没有图片，视频处理的APP来说，对安全，对数据，对APP的崩溃率，对性能关注度不高的APP来说，集约型API是很简单的网络请求方式。

 

2.离散型API的作用：其实就是抽象一层，减少业务层的代码量。对于大型APP，希望能为每个网络请求做更多的定制化策略的APP来说，网络层会提供很多的控制接口给业务APP，通过离散型API设计，就可以让这部分控制放在网络层，而不侵入业务代码。所以可以认为离散型API其实是对网络层请求又做了一层封装而已。

 

3.离散型API的最大好处：就是可以把部分网络策略的代码从业务代码中分离，可以在非业务代码部分做更多的控制。（比如分页处理，比如缓存控制，比如取消哪个请求（之前的请求还是新请求）），比如多参数，对返回数据，对失败结果的AOP监控），离散型API可以说是让业务抽象出了一个请求管理类，以便加以控制。如果是集约型API，对一些策略处理起来，代码就写的很恶心。

 

4.离散型API和集约型API的选择

 1.对于大型APP来说，可见必须要有离散型API请求。

 2.但是对于大多数网络请求来说，也只是简单的网络请求，每个api都强制做一层离散型封装，这增加了太多的重复代码。

​    3.离散型是必须要存在的，为了控制和减少业务层代码。

 4.即使是集约型为了组件化，也应该只依赖网络框架，否则组件化难以实施。

 

结论：集约型和离散型可以并存。但是集约型只能依赖网络库，如果又希望做一些控制，那就必须走离散型封装的方式。如果能够解决不必要的封装问题，可以考虑只用离散型，比如通过模板，或者其他快速的方式。但是如果不能快速生成离散型代码，可以考虑集约和离散并存。

 

 

**二、代理和block**

1.block和代理都可以作为回调，block更灵活，内部可以做各种控制，代码紧凑，写起来顺手。入参返回和失败都在一起。block会对内部对象强引用，而且bock回调通常是类方法或者单例方法，如果控制器释放，block可能还没释放，会导致问题。关于casa提到的block难以追踪不太理解，尤其在网络层这方面block难以追踪吗？

 

2.代理则单独为请求开了一个方法，而且方法名称一致，可以让代码层次更清晰，让整体代码维护性更好。但是对多层次请求，由于代理已经界定了他们各个方法的角色，所以依赖调用写起来就比较难看（结果回调里有请求参数和请求发起）。

3.block对写代码来说更顺手，够直接访问上下文，这样类中不需要存储临时数据，使用 block 的代码通常会在同一个地方，这样读代码也连贯。delegate  更重一些，需要实现接口，它的方法分离开来，很多时候需要存储一些临时数据，另外相关的代码会被分离到各处，没有 block 好读。

 

结论：应该优先使用 block。而有两个情况可以考虑 delegate。有多个相关方法。假如每个方法都设置一个 block, 这样会更麻烦。2. 为了避免循环引用，也可以使用 delegate。

https://www.zhihu.com/question/29023547

 

**三、多服务器场景支持**

1.自身有多个服务器域名调用

对应的api的path也不相同

 

多服务器场景需要支持什么？

每个服务器的baseURl不同

每个服务器的path不同

每个服务器的公共参数可能不同

每个服务器的安全策略也可能不同

每个服务器的请求策略可能也不同

每个服务器成功失败的全局处理可能不同

 

3.因此需要暴露给外界设置多个服务的接口，每个服务目前可能需要包含

baseurl

公共参数字段defaultParams

securityPolicy

requestPolicy

 

结论：初始化的时候，允许根据服务作为key去设置不同服务的全局配置，request中传入服务的key，从而取得不同服务的配置。

 

### 1.1.1功能需求

- Http GET POST UPDATE  DELETE 操作支持
- restful API 访问支持
- 文件上传、下载、进度展示支持
- 异步、同步访问方式支持
- WebSocket支持
- HttpDns支持 (待定)
- 网络数据缓冲
- 网络访问工具集：ping,mock数据，网络连接状态工具 ......
- 网络访问Log管理
- 通用数据结构封装
- 加密、安全、签名、证书验证支持
- 参数和数据校验支持
- 重试机制
- token过期的通知
- 文件校验

### 1.1.2设计需求

   (1)BAFNetwork使用离散型API调用，每个API请求有一个唯一的请求类管理，有利于组件独立和管理。

   (2)BAFNetwork支持block和代理两种response数据回调管理方式。

   (3)BAFNetwork支持通过config配置模块传入公共参数。

   (4)BAFNetwork支持字典入参请求发起方式。

   (5)默认返回服务器下发原始数据给App使用，BAFNetwork提供一个洗数据的reformer机制给App使用。

   (6)为支持多服务器场景，BAFNetwork提供服务器地址设置给APP请求时设置。

   (7)BAFNetwork提供灵活的配置工具，提供给业务App使用，包括默认网络错误提示文案，网络不可达提示等提示配置、环境开关，日志开关配置、请求管理配置、策略配置。

## 1.2 RESTful API

### 1.2.1 RESTful 请求的构成

(1) Request 地址 URL

(2) Request Header 信息

(3) Request 参数

(4) Request 返回结果 (状态, 数据, 通知)

### 1.2.2 RESTful API 方法

(1) GET 获取某资源

(2) PUT/POST 创建某资源

(3) DELETE 删除某资源

(4) PATCH/UPDATE 更新某资源

当前BabyTree的应用中基本使用的是前两种方法，对于更新某资源也使用POST方法

### 1.2.3 同步、异步请求方式的处理

(1) 异步请求方式支持

(2) 同步请求方式支持

 

## 1.3 文件上传、下载

### 1.3.1 文件服务器

(1) 七牛Server：存放图片

(2) 内部Server：配置文件、APP包、音乐，.....

要求：在统一的API基础上，自动选择server进行文件上传下载

### 1.3.2 上传、下载进度管理

提供给APP相关进度的API，供业务APP UI使用

### 1.3.3 批量上传、下载管理

-提供APP文件集合上传

-提供APP文件目录上传

-提供APP文件集合下载

-下载后文件的存储管理

### 1.3.4 上传、下载进度、状态查询

-提供给APP可根据输入的信息查询上传、下载文件状态、进度的API
