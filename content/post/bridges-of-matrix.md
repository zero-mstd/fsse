---
title: Matrix 的两种房间和八种桥
date: 2021-01-24
draft: false
---
今天注册了一个 [Matrix](https://matrix.org/) 玩了玩，最吸引我的是里面“桥（bridge）”的概念，它希望能做到和其他 IM 互通. 我简单地尝试了 Matrix 连 IRC，又仔细阅读了 Matrix 关于这部分的文档，稍稍有了一点自己的理解，在这里简单总结一下. 我阅读的两份材料是：

- Matrix 文档中对桥的简要介绍：<https://matrix.org/bridges/>;
- Matrix 文档中对桥的类型的介绍：<https://matrix.org/docs/guides/types-of-bridging>.

请注意，我这里只是纸上谈兵，没有操作尝试过，只是阅读了这两份材料后的一个个人理解，未必是正确的，写这篇文章是为了得到指点和交流. 另外，我不是按照这两份材料中的方式来理解这个问题的，所以建议先去阅读这两份材料再看我下面写的东西，看看我的理解对不对. 以免我的理解本来就有问题，却被先入为主了.

-----

## 当我们说“桥”的时候我们在说什么？

Matrix 是一个聊天软件，世界上还有其他的聊天软件. 当我们说 Matrix 的桥可以让它与其他聊天软件互通时，互通的是什么东西呢？按我的理解，互通的是房间（room），是房间共享. 那么房间又是什么呢？最简单的一个例子，群组就是一个房间. 无论是 3 个人还是 30 个人还是 300 个人，如果这些人在一起建了一个群组，那么这个群组就是一个房间. 群组这个概念在很多聊天软件里都有，据我所知，Matrix 里有群组，telegram 有群组，IRC 的频道也是一个群组.

那么一对一的私聊是一个房间吗？看上去 Matrix 这里是这样的，Matrix 的 dm 的逻辑就是你开了一个房间然后邀请另一个人进来，这样就是一个私聊了.

## 好的，那房间怎么互通呢？

按我的理解，房间的互通要分三种情况：

- a. 先有其他软件里的远程房间（remote room），如何在 Matrix 里加入那些远程房间？用“镜像”的语言来说就是，Matrix 怎么镜像其他软件的房间？
- b. 先有 Matrix 房间，如何在其他的聊天软件里加入这个 Matrix 房间？用“镜像”的语言来说就是，别的软件怎么镜像 Matrix 的房间？
- c. 无论先后顺序，已经有其他软件上的远程房间和 Matrix 上的房间了，这两个房间能不能合并到一起？

-----

对于情况 a，这个比较简单，Matrix 自己就有桥，这个桥可以透明地（transparently）加入到远程房间里，然后在 Matrix 这边形成一个房间，这个房间叫做一个 **portal room**，Matrix 用户可以加入到这个 portal room 中去. 例子：在 IRC 的 freenode 节点已经有一个频道：##english@freenode.net，那么我可以在 Matrix 中搜索 #freenode_##english:matrix.org，然后点击创建频道. 然后 Matrix 会自动帮我搞一个桥，并显示：This room can't be previewed. Do you want to join it? 点“加入”，于是，就成功进入了这个远程房间. 在 IRC 那边看到的就是 Matrix 的用户名后面加上一个“[m]”标记.

不过没有看到这样接入 telegram 房间的办法，这个可能只对 IRC 的 freenode 有效.

-----

对于情况 b，要用户自己去设置桥. 这样的话，一个 Matrix 房间可以有多个远程房间. 用“镜像”的语言来说就是在不同的软件上有多个镜像. 这种其他软件上的远程的镜像房间，叫做 **plumbed room**. 举个例子，Matrix 自己的官方的在 Matrix 上的房间 #matrix:matrix.org，就被镜像到了 IRC 上（#matrix@freenode.net）和 Slack 上（matrixdotorg/@matrix）.

这里我自然而然想到的一个问题就是：那么被 Plumbed 到 freenode 上的镜像房间能不能再 portal 回去呢？按照规则，我搜索了 #freenode_#matrix:matrix.org，结果是一个大大的感叹号. 我并不清楚这一步是发生了什么.

-----

对于情况 c，就比较复杂了. 第二份材料里有这么一段话：

> Migrating rooms between a portal & plumbed room is currently a bit of a mess, as there’s not yet a way for users to remove portal rooms once they’re created, so you can end up with a mix of portal & plumbed users bridged into a room, which looks weird from both the Matrix and non-Matrix viewpoints. <https://github.com/matrix-org/matrix-appservice-irc/issues/387> tracks this.

我有点没看明白，不知道 plumbed room 指的是被镜像的 Matrix 房间还是我上面说的镜像出去的其他软件里的远程房间. 但无论如何我也不知道为什么会有把 portal room 和 plumbed room 合并的需求？难道不该是把原原本本的 Matrix 房间和原原本本的其他软件里的远程房间合并嘛？没看懂……

-----

未完待续……好难，不想写了……

