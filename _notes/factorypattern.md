---
title: "Factory Pattern"
---

- From [[designpattern]]

> The Factory Pattern is a design pattern in which a factory class is responsible for creating objects of different types based on a given set of parameters.
- This allows for more flexibility in object creation and reduces the amount of duplicate code required.
- The client code asks the factory to create an object without knowing which specific class is being used, and the factory returns an instance of the appropriate class based on the parameters given.

<hr>

Suppose we run a coffee factory producing various coffee products such as americano and latte. Subclass containing recipt of americano, latte and milk would be delivered by conveyor belt and superclass, factory, produces products. 

## Factory Pattern of JavaScript

```js
class Latte
{
    constructor()
    {
        this.name = "latte";
    }
}
class Espresso
{
    constructor()
    {
        this.name = "Espresso";
    }
}
class LatteFactory
{
    static createCoffe()
    {
        return new Latte();
    }
}
class EspressoFactory
{
    static createCoffe()
    {
        return new Espresso();
    }
}
const factoryList = { LatteFactory, EspressoFactory }

class CoffeeFactory
{
    static createCoffe(type)
    {
        const factory = factoryList[type];
        return factory.createCoffe();
    }
}
class main = () => {
    const coffee = CoffeeFactory.createCoffee("LatteFactory");
    console.log(coffee.name);   // latte
}

main();
```

<hr>

## Factory Pattern of Java

```java
abstract class Coffe
{
    public abstract int getPrice();

    @Override
    public String toString()
    {
        return "Hi this coffee is " + this.getPrice();
    }
}
class CoffeeFactory
{
    public static Coffee getCoffe(String type, int price)
    {
        if("Latte".equalsIgnoreCase(type))  return new Latte(price);
        else if("Americano".equalsIgnoreCase(type)) return new Americano(price);
        else
        {
            return new DefaultCoffe();
        }
    }
}
class DefaultCoffee extends Coffee
{
    private int price;
    
    public DefaultCoffe()
    {
        this.price = -1;
    }

    @Override
    public int getPrice()
    {
        return this.price;
    }
}
class Latte extends Coffe
{
    private int price;

    public Latte(int price)
    {
        this.price = price;
    }

    @Override
    public int getPrice()
    {
        return this.price;
    }
}
class Americano extends Coffe
{
    private int price;

    public Americano(int price)
    {
        this.price = price;
    }

    @Override
    public int getPrice()
    {
        return this.price;
    }
}
public class HelloWorld
{
    public static void main(String[] args)
    {
        Coffee latte = CoffeeFactory.getCoffee("Latte", 4000);
        Coffee ame = CoffeeFactory.getCoffee("Americano", 3000);
        System.out.println("Factory latte ::" + latte);
        System.out.println("Factory ame ::" + ame);
    }
}
```