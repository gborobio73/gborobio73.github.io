---
title:  "Building tightly coupled systems using Java 8 default methods"
date:   2017-05-04 15:04:23
categories: [java 8, default methods, coupling, software development]
tags: [java 8, default methods, coupling, software development]
---
<sub>3 min read</sub>

One of the new features of Java 8 is the concept of default methods in interfaces<sup>[ (1)](#fnOne)</sup>.
> “Default methods enable you to add new functionality to the interfaces of your libraries and ensure binary compatibility with code written for older versions of those interfaces”.

This is excellent: you can extend your interfaces without breaking current implementation (backwards compatibility).
As great as this idea is, developers have found another usage for default interfaces: **build tightly coupled systems that look loosely coupled**.

Let’s say you are building some IoT cyborg pets to keep you company: A Duck and an Owl. Your code would look something like this:
{% highlight java %}
public class Duck {}
public class Owl {} 
{% endhighlight %}
Now you want to add a common `fly` behaviour to them, how do you achieve that? By inheritance, of course <sup>[ (2)](#fnTwo)</sup>: You create a base abstract class, add the `fly` behaviour there, and make your cyborgs to extend the base class:
{% highlight java %}
public abstract class FlyingCyborg {	
    public void fly() {
        //implementation goes here e.g. move wings
    }
}

public class Duck extends FlyingCyborg { }

public class Owl extends FlyingCyborg {	}
{% endhighlight %}
Now your both cyborg pets can `fly`, beautiful.

And now you realize it would be great if they could also `run`. But wait, we have a problem: you shouldn’t add `run` behaviour to a FlyingCyborg class, not to break the Single Responsibility Principle<sup>[ (3)](#fnThree)</sup>, and Java doesn’t allow multi-inheritance! Oh, no, horror!

**Default methods to the rescue!**

Yes, thank you Java 8! Now we can work around all these nasty impediments<sup>[ (4) ](#fnFour)</sup>and make your code great again! 

Easy as f**k: we create an interface RunningCyborg with one default method `run`, and make our lovely pets to implement this interface. This way our cyborgs will inherit `run` behaviour:
{% highlight java %}
public interface RunningCyborg {
    default void run() {
        //implementation goes here e.g. move legs, or wheels
    }
}

public class Duck extends FlyingCyborg implements RunningCyborg { }

public class Owl extends FlyingCyborg implements RunningCyborg { }
{% endhighlight %}
Voilà! Our pets can now `fly` and `run`. There’s one optimization though: make FlyingCyborg to implement RunningCyborg and our pets will inherit `run` out of the box. 

Now, your pets will have to sleep, won’t they? 
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

**No, it's not. It’s horrible!** If you haven’t noticed it, I’m being ***ironic***, ***sarcastic***. This is wrong, really wrong. We have built a tightly coupled system<sup>[ (5)](#fnFive)</sup>. Our cyborgs pets are tightly coupled to the abstract class, and to the interfaces; we cannot test them separately: mocking an abstract base classe is hard, and how do you even mock a default method?<sup>[ (6)](#fnSix)</sup>. And how do you test it?

“Favour composition over inheritance”, in case you haven’t heard before<sup>[ (7)](#fnSeven)</sup>.

As weird as all of this might sound, it's happening right now. I’m currently working on a project where default methods are used this way. It's the wild wild west.

<a name="fnOne">(1)</a> [Default methods](https://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)

<a name="fnTwo">(2)</a>	No, of course not.

<a name="fnThree">(3)</a> [Single Responsibility Principle](https://en.wikipedia.org/wiki/Single_responsibility_principle)

<a name="fnFour">(4)</a> [SOLID principles](https://en.wikipedia.org/wiki/SOLID_(object-oriented_design))

<a name="fnFive">(5)</a> [Monolithic application](https://en.wikipedia.org/wiki/Monolithic_application)

<a name="fnSix">(6)</a> There might be a way you can mock a default method, but you would at least need a composite instead of inheritance

<a name="fnSeven">(7)</a> [Composition vs inheritance: how to choose](https://www.thoughtworks.com/insights/blog/composition-vs-inheritance-how-choose)
