---
title: "Strategy Pattern"
---

> A behavioral design pattern that enables an object to dynamically change its behavior by selecting and using an algorithm from a family of interchangeable algorithms

- The pattern consists of defining a set of algorithms(strategies) that can be selected and used at runtime based on the specific requirements of the context

1. Context: The objects that uses a Strategy object
2. Strategy: The interface that defines the algorithm to be implemented
3. Concrete Strategy: The concrete implementation of the Strategy interface

<br>

## Strategy Pattern of Java

```java
import java.text.DecimalFormat;
import java.util.ArrayList;
import java.util.List;

interface PaymentStrategy
{
    public void pay(int amount);
}
class KAKAOCardStrategy implements PaymentStrategy
{
    private String name;
    private String cardNumber;
    private String cvv;
    private String dateOfExpiry;

    public KAKAOCARDStrategy(String nm, String ccNum, String cvv, String expiryDate)
    {
        this.name = nm;
        this.cardNumber = ccNum;
        this.cvv = cvv;
        this.dateOfExpiry = expiryDate;
    }

    @Override
    public void pay(int amount)
    {
        System.out.println(amount + " paid using KAKAOCARD");
    }
}
class LUNACardStrategy implements PaymentStrategy
{
    private String emailId;
    private String password;

    public LUNACardStrategy(String email, String pwd)
    {
        this.emailId = email;
        this.password = pwd;
    }

    @Override
    public void pay(int amount)
    {
        System.out.println(amount + " paid using LUNACard");
    }
}
class Item
{
    private String name;
    private int price;
    public Item(String name, int cost)
    {
        this.name = name;
        this.price = cost;
    }
    public String getName()
    {
        return name;
    }
    public int getPrice()
    {
        return price;
    }
}
class Shopping Cart
{
    List<Item> items;

    public ShoppingCard()
    {
        this.items = new ArrayList<Item>();
    }
    public void addItem(Item item)
    {
        this.items.add(item);
    }
    public void removeItem(Item item)
    {
        this.items.remove(item);
    }
    public int calculateTotal()
    {
        int sum = 0;
        for(Item item : items)
        {
            sum += item.getPrice();
        }
        return sum;
    }
    public void pay(PaymentStrategy paymentMethod)
    {
        int amount = calculateTotal();
        paymentMethod.pay(amount);
    }
}
public class HelloWord
{
    public static void main(String[] args)
    {
        ShoppingCart cart = new ShoppingCart();

        Item A = new Item("itemA", 100);
        Item B = new Item("itemB", 300);

        cart.addItem(A);
        cart.addItem(B);

        cart.pay(new LunaCardStrategy("email", "pwd"));

        cart.pay(new KAKOCardStrategy("email", "pwd"))
    }
}
```