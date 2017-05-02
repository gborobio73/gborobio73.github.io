---
title:  "Building tightly coupled systems using Java 8 default methods"
date:   2017-05-04 15:04:23
categories: [java 8, default methods, coupling, software development]
tags: [java 8, default methods, coupling, software development]
---

One of the new features of Java 8 is the concept of default methods in interfaces. Documentation says that (1) “default methods enable you to add new functionality to the interfaces of your libraries and ensure binary compatibility with code written for older versions of those interfaces”.

This a great idea: you can now extend your interfaces without breaking current implementation (backwards compatibility).
As great as this idea is, developers around the world have found another use for default interfaces: building tightly coupled systems that look loosely coupled.

Let’s have an example: Let’s say you are building some cyborg pets to keep you company: A Duck and an Owl. Your code would look something like this:
{% highlight java %}
public class Duck {}
public class Owl {} 
{% endhighlight %}
Now you want to add fly behaviour to them, and it’s common to both. How do you achieve that? By inheritance, of course (2): You create a base abstract class, add the fly behaviour there and make your cyborgs to extend the base class:
{% highlight java %}
public abstract class FlyingCyborg {	
	public void fly() {
	//Move wings up and down
	}
}

public class Duck extends FlyingCyborg {
}

public class Owl extends FlyingCyborg {	
}
{% endhighlight %}

Now your both cyborg pets can fly, beautiful.

And now you realize it would be great if they could also run. But wait, we have a problem: you shouldn’t add run behaviour to a FlyingCyborg class (not to break the Single Responsibility Principle (3)), and Java doesn’t allow multi-inheritance! Oh, no, horror!

Default methods to the rescue!

Yes, thank you Java 8! Now we can work around all these nasty impediments and make our code great again! 

Easy as f**k: we create an interface RunningCyborg with one default method run, and make our lovely pets to implement this interface. This way our cyborgs will inherit run behaviour:
{% highlight java %}
public interface RunningCyborg {
	default void run() {
		//run cyborg, run
	}
}

public class Duck extends FlyingCyborg implements RunningCyborg {
}
public class Owl extends FlyingCyborg implements RunningCyborg {	
}
{% endhighlight %}

Voilà! Our pets can now fly and run. There’s one optimization though: make FlyingCyborg to implement RunningCyborg and our pets will inherit run out of the box. 

Now, your pets will have to sleep, won’t they? 
{% highlight java %}
public interface SleepingCyborg {
	default sleep() {}
}
[…]
{% endhighlight %}

Beautiful, isn’t it?

No, it is not. It’s horrible! If you haven’t noticed it, I’m being ironic, sarcastic. This is wrong, really wrong. We have built a tightly coupled system. Our cyborgs are deeply coupled to the abstract class and to the interfaces; we cannot test them separately: mocking abstract base classes is hard, and how do you even mock a default method? (5) And test it?

“Favour composition over inheritance”. In case you haven’t heard before (6)

As weird as all of this might sound, I’m currently working on a project where default methods are used this way, and not to provide backwards compatibility. Those classes have no unit tests, of course. 

(1)	https://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html
(2)	No, of course not.
(3)	https://en.wikipedia.org/wiki/Single_responsibility_principle
(4)	https://en.wikipedia.org/wiki/Monolithic_application
(5)	There might be a way you can mock a default method, but you would at least need a composite instead of inheritance
(6)	https://www.thoughtworks.com/insights/blog/composition-vs-inheritance-how-choose