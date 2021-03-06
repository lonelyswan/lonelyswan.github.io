---
layout: post
title: 复杂对象，小心对待
category: Java
description: "创建大型对象的开销往往是很大的，尽量去避免因为创建大量的对象导致的性能是很有必要的..."
modified: 2015-03-25
tags: [java,effective]
image:
  feature: abstract-12.jpg
---


之前看《effective java》时看到过有一原则“避免创建不必要的对象”。

在工作中也遇到过这样的问题，对于比较大的对象，创建往往开销很大，不为了一点简单的需求，创建大量的对象更是需要竭力去避免的。

首先声明，这不是说避免创建对象，创建小对象的开销很小，并不值得我们为了一丁点性能的提升而加大程序的复杂程度，这样反而容易导致错误的发生，会得不偿失。

我在工作中解决一个问题时，曾经发现过类似这创建了大量复杂对象导致的性能问题。问题大致是这样的，在技术升级过程中，为了适配老对象和新对象的匹配未提，提供了一个老对象对新对象的转换函数。这个函数很简单就是使用反射的办法操作新老对象依次复制所有的属性内容，以及转化为新的属性。看似一个没有问题的转换函数造成了性能的巨大损失。

具体的问题是：前端的需求是需要从后端拿到昨天一个很大区域的所有订单并且以点的形式显示在地图上。可是后端从接到请求到返回所有数据竟然需要60秒左右的时间，只有20000左右个订单的经纬度信息，居然耗时那么久。

我开始查找问题的时候首先估计是不是表上哪个字段没加索引，看了下表结构也没有发现问题。接着就自己试着运行了一下那段查找的sql发现返回也只有400ms左右，与60s差别太大，数据库肯定不可能是瓶颈。查了查代码才发现，原来技术改进还没搞完，有些新方法还没有提供，为了适配新对象，从老数据库里取出来的老对象都要进行转换。小对象就算了，不会有什么问题，关键是订单这个对象有70多个字段，方法更是不计其数。算下来一个对象大小得有200K左右。新建20000个200K的复杂对象，这个开销就不得了了。权衡了一下，因为前台只需要4个字段，就把新老对象转换的代码删除掉，换上个vo先用，作为临时性的方案。算是暂时解决了这个巨慢的问题。

这个工作中的小插曲算是一个提醒，以后码代码在操作复杂对象的时候一定要对可能造成的性能问题的地方有个预估。避免这样的情况再次发生。
