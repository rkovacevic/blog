title: Easy singletons in Java
date: 2011-02-08 16:45:00
tags: [java pattern singleton]
---

Singleton is a design pattern used when only one instance of a given class can exist in an application. Although some argue it to be an anti-pattern, it remains one of the most used idioms, so I'm not going to go into if you should use it or not, rather I'll try to show you how to use it properly, if you do decide to use it.  

First, here are some things we want in any singleton:  

*   It should not be possible to create more than one instance of it per application
*   It should be thread safe
*   Since it will be used all over the application, getting the singleton instance should be as fast as possible

Ok, so how do we do it? Most people would probably start with something like this:  

<pre>// Wrong way to do a singleton  
public class NaiveSingleton {  

    private static NaiveSingleton instance;  

    private NaiveSingleton() {  
        // Initialize  
    }  

    public static NaiveSingleton getInstance() {  
        if (instance == null) {  
            instance = new NaiveSingleton();  
        }  
        return instance;  
    }  
}  
</pre>

There are more than few problems with this singleton. Most obvious problem is that the _getInstance_ method is not synchronized, so if two threads enter it around the same time, they could get two different instances, thus breaking our only-one-instance-per-application rule. If we synchronize _getInstance_, we run into a performance problem, now threads have to wait for each other. But even if we get around this problem with some clever hacks, we still need to worry about overriding the _clone_ method, to prevent another instance to be created that way, and all kind of other issues.  

Fortunately, there is a much easier and more robust solution to crate a singleton. Ready? Here it comes:  

<pre>public enum ProperSingleton {  
    INSTANCE;  

    public static ProperSingleton getInstance() {  
        return ProperSingleton.INSTANCE;  
    }  
}</pre>

Since enum constants are guaranteed to have only one instance by the JVM, we don't need to worry about anything, it's all handled for us, and in an optimized way.  

This solution comes from Joshua Bloch's excellent book "[Effective Java](http://java.sun.com/docs/books/effective/)". It's one of the most elegant and beautiful idioms for a common Java problem that I've ever seen. When you see it once, there is really no way you will ever write another singleton any other way.