---
title: "Singleton Pattern"
---

- From [[designpattern]]

> Sigleton patterns are patterns that have only one instance in one class

- Usually used for database connection module, network communication, cache
- Other modules shares the one instance
-> heavy resource
- Pros : Reducing the cost of creating instance as sharing the instance
- Cons : Incresing dependency

## Singletone pattern of JavaScript

In case of JavaScript, every object created by literal {} or new Object is distinct. 

```js
class Singletone {
    constructor() {
        if(!Singleton.instance){
            Singleton.instance = this
        }
        return Singleton.instance
    }
    getInstance() {
        return this.instance
    }
}
const a = new Singletone()
const b = new Singletone()
// a == b is true
```

### Database connection module

```js
const URL = "mongodb://localhost:port//"
const createConnection = url => ({"url" : url})
class DB {
    constructor(url) {
        if(!DB.instance) {
            DB.instance = createConnection(url)
        }
        return DB.instance
    }
    connect() {
        return this.instance
    }
}
const a = new DB(URL)
const b = new DB(URL)
// a==b is true
```

<hr>

## Singletone Pattern of Java

There are 7 ways to implement singleton pattern
1. Eager Initialization
2. Static block initialization
3. Lazy initialization
4. Thread safe initialization
5. Double-Checked Locking
6. **Bill Pugh Solution**
7. **Enum**
The bolded parts indicate the the proven methods

### Eager Initialization
- Due to ```static final```, safe in multi thread environment
- ```static``` makes space resource waste
- No exception handling

```java
class Singleton {
    private static final Singleton INSTANCE = new Singletone();

    private Singletone() {}

    public static Singleton get Instance() {
        return INSTANCE;
    }
}
```

### Static block initialization
- Exception handler using static block
- There is space resource waste still

```java
class Singleton
{
    private static Singleton instance;

    private Singletone() {}

    static
    {
        try
        {
            instance = new Singletone();
        }
        catch (Excpetion e)
        {
            throw new RuntimeException("Error occured during creating Singletone object");
        }
    }
    
    public static Singleton getInstance()
    {
        return instance;
    }
}
```

### Lazy initialization
- Overcome cons of ```static```
- Not Thread safe

```java
class Singleton
{
    private static Singleton instance;

    private Singletone () {}

    public static Singleton getInstance()
    {
        if(instance == null)
        {
            instance = new Singletone();
        }
        return instance;
    }
}
```

#### Problem in multi-thread environment
Java runs in multi-thread environment. This can lead to code execution issues due to concurrency.
1. There are thread A and thread B
2. Thread A pass through if statement and enter the generation code (no initialization)
3. In the same time, thread B pass through if statement. If statement would be true because thread A doesn't execute instance code yet
4. As a result, both of thread A and thread B executes initialization code
Below code prove singleton object is not same with hash code

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executor;

class Singleton
{
    private static Singleton instance;

    private Singleton() {}

    public static Singleton getInstance()
    {
        if(instance == null)
        {
            instance = new Singletone();
        }
        return instance;
    }
}

public class Main
{
    public static void main(String[] args)
    {
        Singleton[] singleton = new Singleton[10];

        ExecutorService sercie = Executors.newCachedThreadPool();

        for(int i = 0 ; i<10 ; i++)
        {
            final int num = i;
            service.submit(() -> {
                singleton[num] = Singleton.getInstance();
            });
        }

        service shutdown();

        for(Singleton s : singleton)
        {
            System.out.println(s.toString());
        }
    }
}
```

### Thread safe initialization

- Let only one thread pass through a method at one time using ```synchronized``` keyword
- performance decreased because of ```synchronized``` method

```java
class Singleton
{
    private static Singleton instance;

    private Singleton() {}

    public static synchronized Singleton getInstance()
    {
        if(instance == null)
        {
            instance = new Singleton();
        }
        return instance;
    }
}
```

### Double-Checked Locking
- Use ```synchronized``` keyword to initializer only
- solve I/O discrepancies problem adding ```volatile``` keyword
- Not perfectly safe from thread safe problem

```java
class Singleton
{
    private static volatile Singleton instance;

    private Singleton() {}

    public static Singleton getInstance()
    {
        if(instance == null)
        {
            synchronized(Singleton.class)
            {
                if(instance == null)
                {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

### **Bill Pugh Solution (LazyHolder)**
- safe in multi-thread environment and Lazy Loading available
-> static inner class holder 
- could be destoyed by client using Reflection API, Serialization and Deserialization

```java
class Singletone 
{

    private Singleton() {}

    private static class singleInstanceHolder 
    {
        private static final Singletone INSTANCE = new Singletone();
    }
    public static synchronized Singletone getInstance() 
    {
        return singletoneInstanceHolder.INSTANCE;
    }
}

public class HelloWorld 
{
    public static void main(String[] args)
    {
        Singletone a = Singletone.getInstance();
        Singletone b = Singletone.getInstance();
        // hash code of a and b are same
    }
}
```
1. SingleInstanceHolder inner class does not load on memory when initializing singleton class becuase of ```static``` keyword
2. ```getInstance()``` returns static member of innerclass. Inner class was initialized once.
3. Prevent to reallocate new value using ```final``` keyword

## Enum
- enum makes member as private and initailize once -> thread safe
- Safe from Reflection attack
- We need to rewrite the whole code if we convert singleton class to multiton class
- Class inheritance is not available except enum

```java
enum SingletonEnum
{
    INSTANCE;

    private final Client dbClient;

    SingletonEnum()
    {
        dbClient = Database.getClient();
    }

    public static SingletonEnum getInstance()
    {
        return INSTANCE;
    }

    public Clinet getClinet()
    {
        return dbClient;
    }
}

public class Main
{
    public static void main(String[] args)
    {
        SingletonEnum singleton = SingletonEnum.getInstance();
        singleton.getClient();
    }
}
```

<hr>

## Singleton pattern of mongoose

```js
Mongoose.prototype.connect = function(uri, options, callback) {
    const _mongoose = this instance of Mongoose ? this : mongoose;
    const conn = _mongoose.connection;

    return _mongoose._promiseOrCallback(callback, cb => {
        conn.openUri(uri, options, err => {
            if(err != null)
                return cb(err);
            return cb(null, _mongoose);
        });
    });
};
```

## Singleton pattern of MySQL
```js
const mysql = require('mysql');
const pool = mysql.createPool({
    connectionLimit: 10,
    host: 'example.org',
    user: 'username',
    password: 'passwd',
    database: 'dbname'
});
pool.connect();

pool.query(query, function(error, results, fields) {
    if(error) throw error;
    console.long("The solition is: ", results[0].solution);
});

pool.query(query, function (error, results, fields){
    if(error) throw error;
    console.log('The solution is: ', results[0].solution);
});
```

<hr>

## Disadvantage of Singleton Pattern
- Hard to make independent instance because singleton pattern uses an already implemented instance.
-> become a problem When we conduct unit test, Test Driven Development(TDD)

<hr>

## DI(Dependency Injection)
> In DI, instead of creating an object or component and managing its dependencies inside the componenet, dependencies are injected from the outside.
- Components do not create their own dependencies, but rather receive them from an external source, such as a container or framework

Suppose we have a 'UserService' that needes a 'UserRepository' to retrieve data fron a database. Instead of instantiating the 'UserRepository' inside the 'UserService', we can inject the dependency from outside.

```java
public interface UserRepository
{
    User getUserById(int id);
}

public class DatabaseUserRepository implements UserRepository
{
    @Override
    public User getUserById(int id)
    {
        // code to retrive user from database
        return user;
    }
}

public class UserService
{
    private final UserRepository userRepository;

    public UserService(UserRepository userRepository)
    {
        this.userRepository = userRepository;
    }
    public User getUserById(int id)
    {
        return userRepository.getUserById(id);
    }
}

public class MyApp 
{
    public static void main(String[] args)
    {
        UserRepository userRepository = new DatabaseUserRepository();
        UserService userService = new UserService(userRepository);

        User user = userService.getUserById(1);
        System.out.println(user);
    }
}
```
- The 'UserService' can now retrieve users from the database without knowing the details of how the data is stored. 
- If we want to switch to a different inplementation of the 'UserRepository', we can simply pass a different object to the 'UserService' constructor.

### Advantage and Disadvantage of DI
Advantage of DI
1. Decoupling: DI helps to decouple components from each other by removing the responsibility of creating and managing dependencies from the components themselves.
2. Testability: DI makes it easier to write unit tests for components by allowing us to replace real dependencies with mock or fake objects.
3. Modularization: DI encourages modular design by promoting the separation of concerns and making it easier to reason about the behavior of individual components

Disadvantage of DI
1. Complexity: DI can introduce additional complexity to the system, especially when used with large or omplex dependency graphs.
2. Runtime errors: DI can lead to runtime errors if dependencies are not correctly configured or injected. 

### Principle of DI
The parent module shouldn't get anything from the child module; both sould reloy on abstractions, and the abstractions shouldn't rely on details.