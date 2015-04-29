---
layout: post
title: 设计模式--策略模式
category: design pattern
description: "策略模式，是设计模式家族中简单，常用的一种模式，与工厂模式有着相似的一面"
modified: 2015-04-29
tags: [design pattern,strategy pattern]
image:
    background: symphony.png
    feature: doraemon.jpg
---

##概念
策略模式属于对象的**行为模式**。其用意是针对一组算法，将每一个算法封装到具有共同接口的独立的类中，从而使得它们可以相互替换。策略模式使得算法可以在不影响到客户端的情况下发生变化。

通俗的来说，同一件事情有不同的做法，客户端并不需要具体的做法是什么，只要指明了要哪种做法，便会按照客户端的要求实施做法。实施办法的改变不会影响到客户端的代码，能够实现具体实现算法与客户端之间的解耦。

举个例子:也比大雄一直深深爱着静香，哆啦A梦给了大雄一些钻石，让大雄用于向静香求婚使用。但是静香是一个傲娇的女孩子，对钻石有颇多要求，形状，重量都要完完全全满足要求，才会答应大雄的求婚。于是大雄就问静香，你想要什么样的钻石。静香说：我要一个圆形的，重量为1克拉的钻石。于是大雄就从哆啦A梦给的钻石中挑出了一颗满足要求的钻石给了静香，满足了她的要求。

在上面这个例子里面，大雄就是Context，持有一个钻石。钻石则作为(Strategy)，这个Abstract Class实现两个接口，大小接口和形状接口。哆啦A梦给的每一颗钻石都是一个具体策略(Concrete Strategy)，继承了Strategy，实现了接口，有具体的实现(有具体的形状和大小)。

静香作为客户端，对大雄(Context)提了一个具体要求，Context就把这个抽象的钻石实例化为一颗满足要求的具体钻石，而静香不需要关心钻石的具体生产过程。就算哆啦A梦又多给了几个钻石，或者是切割了一个钻石，改变了它的形状都和静香没有关系，静香不用关心这些。

###模式意图
将算法封装起来，使得他们可以互相替换，算法的改变不会影响到客户端代码。

###模式参与者

* 上下文(Context): 根据客户端的要求，持有一个由客户端上下文指定的strategy引用。
* 抽象策略(Strategy):一个抽象的对象，通常是一个接口或者是一个抽象类，这一个或多个接口(抽象类)要给出所有具体策略所需要的接口。
* 具体策略(Concrete Strategy):抽象策略的具体实现，封装了具体的算法。

###UML图

<figure>
	<a href="https://raw.githubusercontent.com/lonelyswan/lonelyswan.github.io/master/images/strategy-pattern.jpg"><img src="https://raw.githubusercontent.com/lonelyswan/lonelyswan.github.io/master/images/strategy-pattern.jpg" alt="center"></a>
</figure>