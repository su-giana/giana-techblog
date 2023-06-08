---
title: "Iterator Pattern"
---

- From [[designpattern]]

> Iterator pattern provides a uniform way to traverse the elements of a collection, decoupling the iteration logic from the collection itself and promoting code reusability and maintainability
- you can easily add new collection types without modifying the existing client code that relies on the iterator interface

## Iterator pattern of Java

```java
import java.util.Iterator;

interface Container
// Define a collection interface
{
    Iterator<String> getIterator();
}

// Concrete collection class
class MyContainer implements Container
{
    private String[] elements;

    public MyContainer(String[] elements)
    {
        this.elements = elements;
    }

    @Override
    public Iterator<String> getIterator()
    {
        retur new MyIterator();
    }

    // Concrete iterator class
    private class MyIterator implements Iterator<String>
    {
        private int index;

        @Override
        public boolean hasNext()
        {
            return index < elements.length;
        }

        @Override
        public String next()
        {
            if(this.hasNext())
                return elements[index++];
            return null;
        }
    }
}

public class Main
{
    public static void main(String[] args)
    {
        String elements = {"a", "b", "c", "d", "e"};

        Container container = new MyContainer(elements);

        while(iterator.hasNext())
        {
            String element = iterator.next();
            System.out.println(element);
        }
    }
}
```
