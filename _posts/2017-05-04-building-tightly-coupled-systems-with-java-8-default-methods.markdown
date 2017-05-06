---
title:  "Building tightly coupled systems using Java 8 default methods"
date:   2017-05-02 15:04:23
categories: [java 8, default methods, coupling, software development]
tags: [java 8, default methods, coupling, software development]
---
<sub>3 min read</sub>

One of the new features of Java 8 is the concept of default methods in interfaces<sup>[ (1)](#fnOne)</sup>.
> “Default methods enable you to add new functionality to the interfaces of your libraries and ensure binary compatibility with code written for older versions of those interfaces”.

This is excellent, you can now extend your interfaces without breaking current implementation (backwards compatibility).
As great as this idea is, developers have found another usage for default methods: **build tightly coupled systems that look loosely coupled**.

Let’s say I'm building a couple of IoT cyborg pets to keep me company, a duck and an owl. My code would look something like this:
{% highlight java %}
public class Duck {}
public class Owl {} 
{% endhighlight %}
Now I want to add a common `fly` behaviour to them, how do I achieve this? By inheritance, of course <sup>[ (2)](#fnTwo)</sup>: I create a base abstract class, add the `fly` behaviour there, and make my cyborgs to extend the base class:
{% highlight java %}
public abstract class FlyingCyborg {	
    public void fly() {
        //implementation goes here e.g. move wings
    }
}

public class Duck extends FlyingCyborg { }

public class Owl extends FlyingCyborg {	}
{% endhighlight %}
My cyborg pets can now `fly`. Beautiful.

It would be great if they could also `run`. But wait, I have a problem: I cannot add `run` behaviour to the FlyingCyborg class, or I would break the Single Responsibility Principle<sup>[ (3)](#fnThree)</sup>, and Java doesn’t allow multi-inheritance. Oh no, horror!

**Default methods to the rescue!**

Yes, thank you Java 8! Now I can work around all these nasty impediments<sup>[ (4) ](#fnFour)</sup>and make my code great again! 

I create an interface RunningCyborg with one default method `run`, and I make my lovely pets to implement this interface. This way my cyborgs will inherit `run` behaviour:
{% highlight java %}
public interface RunningCyborg {
    default void run() {
        //implementation goes here e.g. move legs, or wheels
    }
}

public class Duck extends FlyingCyborg implements RunningCyborg { }

public class Owl extends FlyingCyborg implements RunningCyborg { }
{% endhighlight %}
Voilà! My pets can now `fly` and `run`.

Now, they will have to sleep, won’t they? 
{% highlight java %}
public interface SleepingCyborg {
    default sleep() { 
        //implementation goes here e.g. shut systems down
    }
}

public class Duck extends FlyingCyborg implements RunningCyborg, SleepingCyborg { }

public class Owl extends FlyingCyborg implements RunningCyborg, SleepingCyborg { }
{% endhighlight %}
And so on, and so forth. Beautiful, isn’t it?

**No, it's not. It’s horrible!** If you haven’t noticed it, this is me being ***ironic***. This is wrong, really wrong. I have built a tightly coupled system<sup>[ (5)](#fnFive)</sup>. My cyborg pets are tightly coupled to the abstract class and to the interfaces. I cannot test them separately: mocking an abstract base classe is hard and how do I even mock a default method?<sup>[ (6)](#fnSix)</sup>. And how do I test it? My rule of thumb: if you cannot test it, don't write it.

“Favour composition over inheritance”, in case you haven’t heard before<sup>[ (7)](#fnSeven)</sup>.

As weird as all of this might sound, it's happening right now. I’m currently working on a project where default methods are used this way. It's the wild wild west.

---
<a name="fnOne">(1)</a> [Default methods](https://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)<br>
<a name="fnTwo">(2)</a>	No, of course not.<br>
<a name="fnThree">(3)</a> [Single Responsibility Principle](https://en.wikipedia.org/wiki/Single_responsibility_principle)<br>
<a name="fnFour">(4)</a> [SOLID principles](https://en.wikipedia.org/wiki/SOLID_(object-oriented_design))<br>
<a name="fnFive">(5)</a> [Monolithic application](https://en.wikipedia.org/wiki/Monolithic_application)<br>
<a name="fnSix">(6)</a> There might be a way you can mock a default method, but you would at least need composition instead of inheritance<br>
<a name="fnSeven">(7)</a> [Composition vs inheritance: how to choose](https://www.thoughtworks.com/insights/blog/composition-vs-inheritance-how-choose)<br>
