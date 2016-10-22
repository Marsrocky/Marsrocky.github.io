---
layout:     post
title:      "A Scala Primer"
subtitle:   "Scala入门学习"
date:       2016-10-12 21:36
author:     "Jianfei"
header-img: "img/post-scala.png"
tags:        [Code, Scala]
category:   Programming
---

### Scala
- Scala运行于Java平台（Java虚拟机），并兼容现有的Java程序。
- 洛桑联邦理工学院的Martin Odersky于2001年基于Funnel的工作开始设计Scala。
- Twitter, Coursera的后台服务器语言
- 特性：面向对象、函数式编程、静态类型、扩展性、并发性。

### 基本语法

{% highlight scala %}
//Run the interpreter 
//sbt console 

//值，不变量 

val two = 1 + 1

//变量，绑定结果和名称 

var name = "jianfei"

//函数 

def addOne(m: Int): Int = m + 1
//不带参函数，可以省略括号 

def three() = 1 + 2

//匿名函数 

(x: Int) => x + 1
//可以传递匿名函数，同时定义了它的使用名称

val addOne = (x:Int) => x + 1

//用大括号格式化代码 

{i: Int =>
	println("hello world")
	i * 2
}

//部分应用 

def adder(m: Int, n: Int) = m + n
val add2 = adder(2, _:Int)
add2(3)
//会得到5，下划线表示了部分应用了一个函数 

//柯里化函数 

//允许之应用一部分参数 

def multiply(m: Int)(n: Int): Int = m * n
val timesTwo = multiply(2) _

//可变长度参数，可以传多个同类型参数 
//这里对args的每一个arg进行了大写操作 

def capitalizeAll(args: String*) = {
	args.map{ arg =>
		arg.capitalize
	}
}

//类 

class Calculator{
	val brand: String = "Hp"
	def add(m: Int, n: Int): Int = m + n
}

//构造函数 

class Calculator(brand: String) {
  val color: String = if (brand == "TI") {
    "blue"
  } else if (brand == "HP") {
    "black"
  } else {
    "white"
  }
}

//函数和方法在很大程度上是可以互换的。 
//由于函数和方法是如此的相似，你可能都不知道你调用的东西是一个函数还是一个方法。

class  C{
	var acc = 0
	def minc = {acc += 1}
	val finc = {() => acc += 1}
}

val c = new C
c.minc
c.finc

//继承 

class ScientificCalculator(brand: String) extends Calculator(brand) {
  def log(m: Double, base: Double) = math.log(m) / math.log(base)
}

//抽象类 
//你可以定义一个抽象类，它定义了一些方法但没有实现它们。 
//取而代之是由扩展抽象类的子类定义这些方法。你不能创建抽象类的实例。 

abstract class Shape{
	def getArea():Int
}
//重载

class Circle(r: Int) extends Shape{
	def getArea():Int = {r * r * 3}
}
val c = new Circle(2)

//特质(Traits) 
//特质是一些字段和行为的集合，可以扩展或混入（mixin）你的类中。 

trait Car {
	val brand: String
}

trait Shiny {
	val shineRefraction: Int
}

class BMW extends Car{
	val brand = "BMW"
}
//通过with，可以同时扩展多个特质 

class BMW extends Car with Shiny{
	val brand = "BMW"
	val	shineRefraction = 12
}

//类型 
//其实函数也可以是泛型的，来适用于所有类型。 
//当这种情况发生时，你会看到用方括号语法引入的类型参数。 

trait Cache[K, V]{
	def get(key: K): V
	def put(key: K, value: V)
	def delete(key: K)
}
//方法也可以 

def remove[K](key: K)
{% endhighlight %}