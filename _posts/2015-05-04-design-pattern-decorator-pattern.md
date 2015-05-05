---
layout: post
title: 设计模式--装饰模式
category: design-pattern
description: "开发一个类，封装了一个对象的核心操作；而一些非核心的操作，可能会使用，也可能不会使用；现在该怎么办呢？"
modified: 2015-05-04
tags: [design pattern,decorator pattern]
image:
    background: symphony.png
---

##概念

装饰模式属于包装模式，顾名思义是对一个类进行必要的装饰。为什么要装饰呢？

思考一个问题，我们常常会把一些的核心功能抽出来放在一个单独的类中，把这些核心功能封装起来。然而有一些不常用，也不重要的功能，该怎么让这个类去实现呢？

1. 假如说我们把这些并非核心的功能封装在核心的类中，那这些核心功能与非核心功能混杂在一起就失去了“核心”的效果。
2. 如果使用继承的办法，需要哪些功能就新建一个类，继承核心类。假如说功能很多，并且功能之间也需要进行组合，那么我们会被无数的继承出来的类折磨的焦头烂额。就算这样也做不到非核心功能之间的自由组合，代码也没有办法很好的复用。

上帝说，要有光。于是，装饰模式出现了。

举个例子：大雄和静香打算出去约会，对于第一次约会大雄很是紧张，穿什么衣服赴约，令他很苦恼，他干脆不想去了。哆啦A梦拿出了临时克隆人机，打算帮助大雄克隆几个自己。大雄这下就放下心来了，但是一个克隆大雄只能穿一套衣服，穿短袖，牛仔裤要克隆一个大雄出来，穿休闲西装，牛仔裤，又要克隆一个大雄出来。大雄小小的屋子几分钟遍塞满了，这还没有给临时克隆人穿鞋呢。

大雄鼓足了勇气，还是决定亲自去赴约。他先穿了漂亮的T恤，又穿了好看的短裤，最后穿上漂亮的皮鞋。看起来帅气极了～于是大雄高高兴兴的去赴约了。

上面的例子中，克隆的大雄“们”就是对核心类不断继承产生的窘境，大雄(Concrete Component)是一个具体的人类(Component)，而短裤，T恤，鞋子(Concrete Decorator)都属于服饰(Decorator)。

这就是装饰模式，就像各种各样的服饰穿在身上。

###模式意图

动态的给一个对象添加一些额外的职责，添加的功能不会影响到原来的对象。就增加功能来说，Decorator模式相比生成子类更为方便，灵活。

###模式参与者

* 抽象构件（Component）：定义一个对象接口，可以给这些对象动态地添加职责。
* 具体构件（Concrete Component）：定义了一个具体的对象，也可以给这个对象添加一些职责。
* 装饰（Decorator）：抽象装饰类。
* 具体装饰（Concrete Decorator）：具体的装饰对象，起到给Component添加职责的功能。

###UML图

<figure>
	<a href="https://raw.githubusercontent.com/lonelyswan/lonelyswan.github.io/master/images/decorator-pattern.jpg"><img src="https://raw.githubusercontent.com/lonelyswan/lonelyswan.github.io/master/images/decorator-pattern.jpg" alt="center"></a>
</figure>

###模式实现

首先实现Component接口，作为抽象出的具体构建，以及用于Decorator实现
{% highlight java %}
public interface Component{
	public void show();
}
{% endhighlight %}

具体的构建实现Component接口
{% highlight java %}
public class Nobi implement Component{
	private Age age;
	private Height height;
	public setAge(Age age){
		this.age =age;
	}
	public setHeight(Height height){
		this.height =hright;
	}
	public void show(){
		do something;
	}
}
{% endhighlight %}

再实现Decorator抽象类
{% highlight java %}
public abstract class Decorator implement Component{
	private Component component;
	public Decorator(Component component){
		this.component = component;
	}
	@Override
	public void show(){
		component.show();
	}
}
{% endhighlight %}

实现几个具体装饰类
{% highlight java %}
public class Tshirt extends Decorator{
	private Component component;
	public Tshirt(Component component){
		this.component = component;
	}
	public void showTshirt(){
		show Tshirt;
	}
}
public class Shorts extends Decorator{
	private Component component;
	public Shorts(Component component){
		this.component = component;
	}
	public void showShorts(){
		show Shorts;
	}
}
public class Shoes extends Decorator{
	private Component component;
	public Shoes(Component component){
		this.component = component;
	}
	public void showShoes(){
		show Shoes;
	}
}
{% endhighlight %}


###模式优缺点
优点

1. 装饰模式与继承关系的目的都是要扩展对象的功能，但是装饰模式可以提供更多的灵活性
2. 通过使用不同的具体装饰类以及这些装饰类的排列组合，可以创造出很多不同行为的组合

缺点

1. 装饰模式相比继承产生较少的类，但同时生成更多的对象 并且这些对象看上去都比较相似
2. 装饰模式比继承更加容易出错


###装饰模式与其他模式的对比
装饰模式和适配器模式
* 装饰模式——保持端口，增强所考虑对象的功能
* 适配器模式——改变所考虑对象的接口，而不一定改变对象的功能
 
装饰模式和策略模式
* 装饰模式——表皮任意更换，保持核心，要求Component尽量轻
* 策略模式——保持接口不变，使具体算法可以互换，要求抽象策略类尽量重
 
装饰模式和合成模式
* 装饰模式常常用于合成模式的行为扩展上，是集成的替代方案。