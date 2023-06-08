---
title: "Observer Pattern"
---

- From [[designpattern]]

> The observer pattern is a design pattern in which an object, called the **subject**, maintains a list of its dependents, called **observers**, and notifies them automatically of any state change, usually by calling one of their method.
- Commonly used in event driven systems, MVC(Model-View-Controller) pattern
- The observer pattern promotes loose coupling between objects, which can make a system more flexible, reusable, and maintainable

## Observer Pattern of Java

```java
import java.util.ArrayList;
import java.util.List;

interface Subject
{
    public void register(Observer obj);
    public void unregister(Observer obj);
    public void notifyObservers();
    public Object getUpdate(Observer obj);
}

interface Observer
{
    public void update();
}

class Topic implements Subject
{
    private List<Observer> observers;
    private String message;

    public Topic()
    {
        this.observers = new ArrayList<>();
        this.message = "";
    }
    @Override
    public void register(Observer obj)
    {
        if(!observers.contains(obj))    observers.add(obj);
    }
    
    @Override
    public void unregister(Observer obj)
    {
        if(observers.contains(obj))     observers.remove(obj);
    }

    @Override
    publid void notifyObservers()
    {
        this.Observers.forEach(Observer::update());
    }

    @Override
    public Object getUpdate(Observer obj)
    {
        return this.message;
    }

    public void postMessage(string msg)
    {
        System.out.println("Message sent to Topic: " + msg);
        this.message = msg;
        notifyObservers();
    }
}

class TopicSubscriber implements Observer
{
    private String name;
    private Subject topic;

    public TopicSubscriber(String name, Subject topic)
    {
        this.name = name;
        this.topic = topic;
    }

    @Override
    public void update()
    {
        String msg = (String) topic.getUpdate(this);
        System.out.println(name + ":: got message >>" + msg);
    }
}

public class HelloWorld
{
    public static void main(String[] args)
    {
        Topic topic = new Topic();
        Observer a = new TopicSubscriber("a", topic);
        Observer b = new TopicSubscriber("b", topic);
        Observer c = new TopicSubscriber("c", topic);
        topic.register(a);
        topic.register(b);
        topic.register(c);

        topic.postMessage("msg");
    }
}
```

<hr>

## Observer Pattern of JavaScript
In JavaScript, we could implement observer pattern with proxy object
### Proxy object
> A proxy object is a surrogate or placeholder for another object, which allows you to control access to the original object or add additional functionality without changing its interface.
- It intercepts all calls to the original object and can perform operations such as caching results, logging, validating inputs, or restricting access based on permisson
- Proxy object has two paramters
target : target to proxy
handler : function which contains a set of actions intercepting action of the target and defining action of after work

```js
// proxy object
const handler = {
    get: function(target, name){
        return name == 'name' ? `${target.a} ${target.b}` : target[name];
    }
}
const p = new Proxy({a: 'targetname', b: 'username'}, handler);
console.log(p.name);    // 'targetname username;
```

### Observer Pattern using Proxy Pattern

```js
function createReactiveObject(target, callback)
{
    const proxy = new Proxy(target, {
        set(obj, prop, value)
        {
            if(value != obj[prop]){
                cont prev = obj[prop];
                obj[prop] = value;
                callback(`${prop} changed fronm ${prev} to ${value}`);
            }
            return true;
        }
    })
    return proxy;
}
const a = {
    "1 + 1": "2";
}
const b = createReactiveObject(a, console.log);
b.1 + 1 = "2";
b.1 + 1 = "3";
// 1+1 changed from 2 to 3
```