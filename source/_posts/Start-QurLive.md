---
title: 开始啦！！ QurLive
tags: 
- 前端
- 设计
- 后端
- 移动
- 直播
categories:
- course
- 应用
---

大家好，我是之前在贴吧上说要做直播平台的那个小朋友，现在它来了，最近真的挺忙的说。提前说明：要是有什么说得不好的地方，请一定联系我。

# 开始
为什么要做直播平台呢🧐，其实我在想的时候也不大清楚， 总之呢， 涉及多种知识， 有一定技术含量， 不错 ， 那就做吧， 反正当学习， 我也不是那种上来就说一大堆道理和理论的小朋友， 直播是什么，效果怎么样大家都清楚，那就直接开始实践了吧。

- - - -
# 小说两句
有一个问题，那就是最终做成的应用是运行在什么平台，具体要实现什么功能，以及给它取个响当当的名字。

## 技术方面
客户端平台呢，是 iOS， Android，至于技术呢，是 react-native， 当然后续也许会实现等效的原生客户端，然后是桌面端，以及web端， 服务端端会用到 nodejs 和 Golang， 数据库是 Mongodb， 就这样。

## 产品方面
是大是小，是团队是个人，它总归是个项目，它的名字叫做 QurLive， 是一个开源的直播平台，提供完整的开发过程的说明，项目地址： [QurLive](https://github.com/hello-acuario/QurLive), 个人制作，期待您的支持，和对我这个小朋友的照顾🙃。具体的功能：实现视频直播（推流拉流），其中有录制屏幕的，还有来自摄像机的，包括支持OBS等软件等推流等，实现礼物🎁系统、支付系统，即时通讯实现聊天和弹幕等，并优化以达到高性能，当然还有更多有趣等功能等待我们去探索。

- - - -
# 正式开始
在开始写代码之前，我准先制作图标，让QurLive有个门面和形象

dang～

![](Icon.png)

图标里稍稍透出一点理念，不过先卖个关子呀。
项目的UI资源以及源文件都整整齐齐地放在项目的assets分支下。

## 架构方面
我计划在服务端使用偏微服务的架构，好处在于各个功能模块分开部署，甚至可以非常方便地增加和替换某个模块

## 接下来开始工作吧
期间有什么东西需要准备包括图标等，以及原型交互设计之类的，都用到在制作，关于直播的核心技术以及达到高性能或者引入各种玄学，都放到后期制作，目前的首要任务是快速实现一个可用的QurLive。

## 准备一个简单的直播服务器

### 流程
首先我计划先实现最核心的直播功能，先支持的是rtmp协议，在可以进行正常推流拉流之后，再进行优化，接着加入别的协议，例如hls。\
为了检查完成的工作，在这里设定一些指标来辅助检查工作以及完成度:
- [ ] 实现直播最基础的工作
- [ ] 实现完美的音画同步
- [ ] 实现低延迟（不包括对多观看端以及超多观看端的测试）
- [ ] 计算延时
- [ ] 支持比特率更换
- [ ] 实现简易html的播放器

### node 版本
这里就直接使用一个造好的轮子，毕竟一开始一个还什么都没有的项目摆在那里，不能总着什么都自己写对吧？这里使用的是 [Node-Media-Server](https://github.com/illuspas/Node-Media-Server/blob/master/README_CN.md)

按照官方说明运行好程序，该库也支持了docker，真·一键运行，这样就直接实现了我们上述的第一条和第二条，过程非常简单，不过要不断优化，在这之前我们需要先测试它的延时（这里还没有引入延时计算，于是采用简单的看时钟）\
我将该服务部署到了公网服务器，毕竟这样比localhost更有代表性，然后使用obs推流，obs流设置：

![](obsConfig.png) 

在点击开始推流之后obs的显示状态：

![](obsState.png)

一行node解决测试

![](test.png)

接下来我们只要对比就好了
这里使用vlc放的，截图有点粗糙不要介意

![](testResult.png)

可见延迟在5秒左右，而且是音画同步，在这之前我用手机测试了一遍，不过QuickTime出了一点问题，没办法把手机和电脑摆在一起录，不过在用手机测试的时候，使用的是vlc的iOS客户端，延迟2s（这里仍然是单观看设备的）
