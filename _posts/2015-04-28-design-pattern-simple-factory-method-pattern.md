---
layout: post
title: 设计模式--简单工厂模式
category: design pattern
description: "简单工厂模式又叫 static factory method，是工厂模式家族中最简单的实现，但是这种最简单的实现往往却很实用"
modified: 2015-04-28
tags: [design pattern,fatcory method]
image:
    background: symphony.png
---

##概念

简单工厂模式是**创建者模式**的一种，也是一种特殊的工厂模式。简单工厂模式是由一个工厂对象决定创建出哪一种产品类的实例。简单工厂模式是工厂模式家族中最简单实用的模式是工厂模式的一个特殊实现。

大雄被胖虎欺负了，回家找多拉A梦哭诉：我被打了，我好委屈，我要一个“少爷保镖”。这时候能哆啦A梦就从他的口袋里，“噹噹噹噹”掏出一个“少爷保镖”，给了大雄。过了两天，大雄又被胖虎欺负了，回家找哆啦A梦哭诉：我被打了，我要一个“报仇机器人”，这时候能哆啦A梦又从他的口袋里，“噹噹噹噹”掏出一个“报仇机器人”，给了大雄。

在这个例子里面，哆啦A梦就是一个简单工厂，生产一系列“报仇”产品给大雄使用。大雄并不知道“少爷保镖”和“报仇机器人”的生产过程，只是按照需求直接拿到了这个产品。

###模式意图

简单工厂模式根据提供给工厂的要求(数据)，返回几个可能类中的一个类的实例。

###模式参与者

* 工厂(Factory): 接受客户请求，按照请求返回给客户所要求的对象(上例中的：哆啦A梦)
* 抽象产品(Abstract Product): 是工厂模式所创建对象的父类或是共同拥有的接口。可是抽象类或接口。(上例中的：复仇系列产品)
* 具体产品(Concrete Product): 工厂模式所创建的对象都是这个角色的实例。(上例中的：“少爷保镖”，“复仇机器人”)

###UML图

<figure>
	<a href="https://raw.githubusercontent.com/lonelyswan/lonelyswan.github.io/master/images/simple-factory-method.jpeg"><img src="https://raw.githubusercontent.com/lonelyswan/lonelyswan.github.io/master/images/simple-factory-method.jpeg" alt=""></a>
	<figcaption>UML</figcaption>
</figure>


