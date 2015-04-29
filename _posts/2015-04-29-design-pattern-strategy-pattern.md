---
layout: post
title: 设计模式--简单工厂模式
category: design pattern
description: "简单工厂模式又叫 static factory method，是工厂模式家族中最简单的实现，但是这种最简单的实现往往却很实用"
modified: 2015-04-28
tags: [design pattern,fatcory method]
image:
    background: symphony.png
    feature: doraemon.jpg
---

##概念

简单工厂模式是**创建者模式**的一种，也是一种特殊的工厂模式。简单工厂模式是由一个工厂对象决定创建出哪一种产品类的实例。简单工厂模式是工厂模式家族中最简单实用的模式是工厂模式的一个特殊实现。

大雄被胖虎欺负了，回家找多拉A梦哭诉：我被打了，我好委屈，我要一个“少爷保镖”。这时候能哆啦A梦就从他的口袋里，“噹噹噹噹”掏出一个“少爷保镖”，给了大雄。过了两天，大雄又被胖虎欺负了，回家找哆啦A梦哭诉：我被打了，我要一个“报仇机器人”，这时候能哆啦A梦又从他的口袋里，“噹噹噹噹”掏出一个“报仇机器人”，给了大雄。

在这个例子里面，哆啦A梦就是一个简单工厂，生产一系列“报仇”产品给大雄使用。大雄并不知道“少爷保镖”和“报仇机器人”的生产过程，只是按照需求直接拿到了这个产品。

###模式意图

简单工厂模式根据提供给工厂的要求(数据)，返回几个可能类中的一个类的实例。

###模式参与者

* 抽象产品(Abstract Product): 是工厂模式所创建对象的父类或是共同拥有的接口。可是抽象类或接口。(上例中的：复仇系列产品)
* 具体产品(Concrete Product): 工厂模式所创建的对象都是这个角色的实例。(上例中的：“少爷保镖”，“复仇机器人”)
* 工厂(Factory): 接受客户请求，按照请求返回给客户所要求的对象(上例中的：哆啦A梦)

###UML图

<figure>
	<a href="https://raw.githubusercontent.com/lonelyswan/lonelyswan.github.io/master/images/simple-factory-method.jpg"><img src="https://raw.githubusercontent.com/lonelyswan/lonelyswan.github.io/master/images/simple-factory-method.jpg" alt="center"></a>
</figure>

###模式实现

首先是抽象产品
{% highlight java %}
//Abstract Product:
public abstract class RevengeProduct{
	String name;
	String discribe;
	public abstract void revenge();
}
{% endhighlight %}


然后是具体产品
{% highlight java %}
//Concrete Product A:
public class BodyGuard extends RevengeProduct{
	String name;
	String discribe;
	public void revenge(){
	    dorevenge();
	}
}
//Concrete Product B:
public class RevengeRobot extends RevengeProduct{
	String name;
	String discribe;
	public void revenge(){
	    dorevenge();
	}
}
{% endhighlight %}


然后是工厂
{% highlight java %}
//Factory
public Class Factory{
	pulic static RevengeProduct create(String command){
		if(command.equals(bodyGuard))
			return new BodyGuard();
		if(command.equals(revengeRobot))
			return new RevengeRobot();
	}	
}
{% endhighlight %}

###模式优缺点
优点：工厂类含有必要的判断逻辑，可以决定在什么时候创建哪一个产品类的实例，客户端可以免除直接创建产品对象的责任，而仅仅"消费"产品。简单工厂模式通过这种做法实现了对责任的分割。

缺点：

1.当产品有复杂的多层等级结构时，工厂类只有自己，以不变应万变，就是模式的缺点。因为工厂类集中了所有产品创建逻辑，一旦不能正常工作，整个系统都要受到影响。

2.系统扩展困难，一旦添加新产品就不得不修改工厂逻辑，有可能造成工厂逻辑过于复杂,违背了"开放--封闭"原则(OCP)。另外，简单工厂模式通常使用静态工厂方法，这使得无法由子类继承，造成工厂角色无法形成基于继承的等级结构。



