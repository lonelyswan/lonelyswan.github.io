---
layout: post
title: SOLID设计原则
category: design-pattern
description: "无论是24种常用设计模式，还是1000种设计方法，他们都是这SOLID五大设计原则的具体表现..."
modified: 2015-05-10
tags: [design pattern,design principle]
image:
    background: symphony.png
---



作为一个想好好学习设计模式的同学，我打算先放一放具体的设计模式，对设计模式的几大原则进行一次深入的思考和理解。在页尾有几篇Uncle Bob非常不错的文章可以和大家共享。

我写下这篇文章，讲讲我对这几大原则的理解，顺便也为自己复习复习。

###SOLID

首先搞搞清楚SOLID代表了啥吧.

* S代表SRP(单一责任原则)，The Single Responsibility Principle。A class should have one, and only one, reason to change。
* O代表OCP(开放封闭原则)，The Open Closed Principle。You should be able to extend a classes behavior, without modifying it。
* L代表LSP(里氏替换原则)，The Liskov Substitution Principle。Derived classes must be substitutable for their base classes。
* I代表ISP(接口分离原则)，The Interface Segregation Principle。Make fine grained interfaces that are client specific.
* D代表DIP(依赖倒置原则)，The Dependency Inversion Principle。Depend on abstractions, not on concretions.

###S--单一责任原则

简单来说，引起类变化的因素只有一个。一个类只有一个职责，如果一个类有多余一个职责，那么就应该拆分这个类。如果类包含多个职责，代码会变得耦合；SRP看起来是把事物分离成分子部分，以便于能被复用和集中管理，这点也同样适用于方法。

###O--开放封闭原则
简单来说，允许扩展，不许修改。“允许扩展”指的是设计类时要考虑到新需求提出时类可以增加新的功能。“不许修改”指的是一旦一个类开发完成，除了改正bug就不再修改它。通常来说可以通过依赖关系的抽象实现开闭原则，比如接口或抽象类而不是具体类。通过创建新的类实现接口来增加功能。在项目中应用OCP原则可以限制代码的更改，一旦代码完成，测试和调试之后就很少再去更改。这减少了给现有代码引入新bug的风险，增强软件的灵活性。

###L--里氏替换原则
里氏替换原则适用于继承层次结构，指出设计类时客户端依赖的父类可以被子类替代，而客户端无须了解这个变化。因此，所有的子类必须按照和他们父类相同方式操作。子类的特定功能可能不同，但是必须符合父类的预期行为。要成为真正的行为子类型，子类必须不仅要实现父类的方法和属性，也要符合其隐含行为。一般来说，如果父类型的一个子类型做了一些父类型的客户没有预期的事情，那这就违反LSP。比如一个派生类抛出了父类没有抛出的异常，或者派生类有些不能预期的副作用。基本上派生类永远不应该比父类做更少的事情。

一个违反LSP的典型例子是Square类派生于Rectangle类。Square类总是假定宽度与高度相等。如果一个正方形对象用于期望一个长方形的上下文中，可能会出现意外行为，因为一个正方形的宽高不能(或者说不应该)被独立修改。

###I--接口分离原则
接口隔离原则指出客户不应该被强迫依赖于他们不使用的接口。当我们使用非内聚的接口时，ISP指导我们创建多个较小的内聚度高的接口。

当你应用ISP时，类和他们的依赖使用紧密集中的接口通信，最大限度地减少了对未使用成员的依赖，并相应地降低耦合度。小接口更容易实现，提升了灵活性和重用的可能性。由于很少的类共享这些接口，为响应接口的变化而需要变化的类数量降低，增加了鲁棒性。

###D--依赖倒置原则
依赖反转原则(Dependency Inversion Principle,DIP)指出高层次模块不应该依赖于低层次模块；他们应该依赖于抽象。第二，抽象不应该依赖于细节；细节依赖于抽象。方法是将类孤立在依赖于抽象形成的边界后面。如果在那些抽象后面所有的细节发生变化，那我们的类仍然安全。这有助于保持低耦合，使设计更容易改变。DIP也允许我们做单独测试，比如作为系统插件的数据库等细节。

一个程序依赖于Reader和Writer接口，Keyboard和Printer作为依赖于这些抽象的细节实现了这些接口。CharCopier是依赖于Reader和Writer实现类的低层细节，可以传入任何实现了Reader和Writer接口的设备正确地工作

###总结
这些原则就是设计模式的基础原则，虽然现在可能还不能深刻的理解，但是作为学习设计模式的基础，先了解，再慢慢学习各种设计模式的例子，最后可以菜能彻底明白这几个原则的意义所在。



<div markdown="0"><a href="http://www.objectmentor.com/resources/articles/Principles_and_Patterns.pdf" class="btn btn-info">Design Principles and
Design Patterns</a></div>

<div markdown="0"><a href="http://butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod" class="btn btn-info">The Principles of OOD</a></div>


