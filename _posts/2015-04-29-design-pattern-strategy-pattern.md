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

[^1]: <http://xiewenbo.iteye.com/blog/1294207>
[^2]: <http://blog.csdn.net/tjb_1216/article/details/5631143>


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

###模式实现

Strategy要实现的两个interface
{% highlight java %}
//两个interface:
public interface WeightInterface{
	public Weight getWeight();
}
public interface ShapeInterface{
	public Shape getShape();
}
{% endhighlight %}

策略(Strategy)
{% highlight java %}
//Strategy:
public abstract class StrategyDiamond implement WeightInterface,ShapeInterface{
	public Weight getWeight(){}
	public Shape getShape(){}
}
{% endhighlight %}

具体策略(Concrete Strategy)
{% highlight java %}
//Concrete Strategy:
public class DiamondA extends StrategyDiamond{
	Weight weight;
	Shape shape;
	public Weight getWeight(){
		return this.weight;
	}
	public Shape getShape(){
		return this.shape;
	}
}
public class DiamondB extends StrategyDiamond{
	Weight weight;
	Shape shape;
	public Weight getWeight(){
		return this.weight;
	}
	public Shape getShape(){
		return this.shape;
	}
}
public class DiamondC extends StrategyDiamond{
	Weight weight;
	Shape shape;
	public Weight getWeight(){
		return this.weight;
	}
	public Shape getShape(){
		return this.shape;
	}
}
{% endhighlight %}

上下文，持有Strategy的对象(Context)
{% highlight java %}
public class Context{
	private StrategyDiamond diamond;

	public Context(StrategyDiamond req){
		this.diamond = req;
	}

	public Weight getWeight(){
		return this.diamond.getWeight();
	} 
	public Shape getShape(){
		return this.diamond.getShape();
	} 
}
{% endhighlight %}

###模式优缺点

策略模式有很多优点和缺点。[^1]

它的优点有： 

1. 策略模式提供了管理相关的算法族的办法。策略类的等级结构定义了一个算法或行为族。恰当使用继承可以把公共的代码移到父类里面，从而避免重复的代码。 

2. 策略模式提供了可以替换继承关系的办法。继承可以处理多种算法或行为。如果不是用策略模式，那么使用算法或行为的环境类就可能会有一些子类，每一个子类提供一个不同的算法或行为。但是，这样一来算法或行为的使用者就和算法或行为本身混在一起。决定使用哪一种算法或采取哪一种行为的逻辑就和算法或行为的逻辑混合在一起，从而不可能再独立演化。继承使得动态改变算法或行为变得不可能。 

3. 使用策略模式可以避免使用多重条件转移语句。多重转移语句不易维护，它把采取哪一种算法或采取哪一种行为的逻辑与算法或行为的逻辑混合在一起，统统列在一个多重转移语句里面，比使用继承的办法还要原始和落后。

策略模式的缺点有：

1. 客户端必须知道所有的策略类，并自行决定使用哪一个策略类。这就意味着客户端必须理解这些算法的区别，以便适时选择恰当的算法类。换言之，策略模式只适用于客户端知道所有的算法或行为的情况。 

2. 策略模式造成很多的策略类。有时候可以通过把依赖于环境的状态保存到客户端里面，而将策略类设计成可共享的，这样策略类实例可以被不同客户端使用。换言之，可以使用享元模式来减少对象的数量。 


###与简单工厂模式的异同

相同之处：[^2]

1. 它们都有一个抽象类或公共接口，并且在抽象类或者接口中，定义一个方法（或虚拟抽象方法）；

2. 通过子类进行继承父类或者实现接口方法。

3. 使用多态特性，进行实例方法调用，调用的是子类的方法；

 
区别之处：

1. 简单工厂模式 强调的创建类对象，根据 字符串类型参数传入参数，进行实例化；

2. 简单工厂模式，必须定义一个制造实例的工厂类Factory，且其工厂类，返回父类类型，不是子类类型；

3. 策略模式强调的算法封装，而算法的不同，增加相应的子类进行实现。

4. 必须有一个Content类，提供两个方法，一个接受各个算法的实例对象，一个是对子类方法的封装，提供访问客户端统一的接口；

5. 策略模式中接受算法实例的方法，可以结合简单工厂模式，传入字符串参数，内部进行实例化，降低和客户端的耦合；



