---
layout: post
title: Effective Java Notes
date: 2023-06-27 11:12:00-0400 
description: Notes of the book that I was suggested while interning in Sprinklr
tags: coding, java
categories: coding, java
---

I recently started to read the book Effective Java, to improve my coding habits and to learn in-depth about the functionalities provided by Java. Here I would be adding notes of the items that I think are relevant could be helpful to me in future and might to others also. 

## Builders

When the class has a lot of optional parameters, one should consider using Builder. Builders allow you the flexibility which Static Factories and Constructors don’t for optional parameters. 

Traditionally the design pattern of **[telescoping constructor](https://www.captaindebug.com/2011/05/telescoping-constructor-antipattern#:~:text=The%20Telescoping%20Constructor%20is%20an,there%20are%20better%20alternatives%20availble.)** was used in case of optional parameters. But that makes you write a lot of redundant code, which later can become unreadable with many methods of the same name. 

Another method which can be used to tackle the problem of optional parameters is that of using `setter` methods. Here we can use a constructor to just create an object and then use the setter methods to assign attributes to that object. Something like

```java
class Test {
	private int a1;
	private int a2;
	//setters
	public void seta1(int val) {a1 = val;}
	public void seta2(int val) {a2 = val;}
}

//inside main
Test test = new Test();
test.seta1(1);
```

This is the ********JavaBean******** pattern. But this strategy has its own disadvantages, first being that the object would be in an inconsistent state for sometime, and second being that it automatically precludes the possibility of the class being immutable.

Hence to cover for the advantages come builder classes. There syntax is as follows:

```java
class Test {
	private int a1;
	private int a2;
	private int a3;

	public static class Builder {
		//required
		private final int a1;

		//optional
		private int a2 = 0;
		private int a3 = 0;

		public Builder(int a1) {
			this.a1 = a1
		}

		public Builder a2(int val) {
			a2 = val; return this;
		}
		
		public Builder a3(int val) {
			a3 = val; return this;
		}
		
		public Test build() {
			return new Builder(this);
		}
	}

	private Test(Builder builder) {
		a1 = builder.a1;
		a2 = builder.a2;
		a3 = builder.a3;
	}
}

//inside main
Test test = new Test.Builder(a1).a2(a2).build()
```

Using this pattern, instead of making the object directly, the client calls a constructor with all of the required parameters and gets a builder object. Using this builder object, we get access to setter methods which returns the same builder object but after setting some parameters of the object for which the setter method was invoked. Then finally the `build` methods is called which returns the desired class’s object.

This is a substitute for the named optional parameters in Python and Scala.

`Builder` functionality is also well suite to class hierarchies. I’ll come back to this when I have covered *recursive type parameter* and `clone()`

References:

- [Captain Debug's Blog: The Telescoping Constructor (Anti)Pattern](https://www.captaindebug.com/2011/05/telescoping-constructor-antipattern#:~:text=The%20Telescoping%20Constructor%20is%20an,there%20are%20better%20alternatives%20availble.)

## Static Factory Methods

Static factory methods in Java are basically like constructors but carry a meaningful name to them. They are static methods that return  an instance of the native class. These methods can come in handy when you need to have multiple constructors that return the object but with some form of different configuration.

<aside>
💡 In case you forgot, `static` methods don’t need an object of a class to be called. Thus these static factory methods come in handy.

</aside>

These methods have the following advantages over constructors -

1. Static Factory methods allow reuse of already created instances or use cached instances of a class, instead of creating new ones. This can lead to improve in performances. An example of this is is `Boolean.valueOf(boolean var)`
2. It can also return an object of the subtype thus giving you more flexibility over choice of the class of the object returned. This also allows to return object of classes without making those classes public.
3. As I had mentioned earlier, another advantage is that object can vary from call to call as function of the input parameters. `Enum` class makes use of this advantage and thus does not have any public constructors.