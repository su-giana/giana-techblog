---
title: "Database in Spring"
---

- From [[spring]]

## 1. Add Database Dependency
> Database Dependency includes the appropriate database driver dependency in project's build configuration file (e.g., '**pom.xml**', '**build.gradle**')

Add two dependnecy in project's <dependencies> in '**pom.xml**'.

```xml
<dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
    </dependency>
```

<hr>

## 2. Add code

### Use Entity annotation
In H2, JPA annotation is used becuase H2 is JPA compatible DB. Add **@Entity** annotation in Coffee class which is *Persistable Entity* and **@Id** annotation in id field to set id variable to ID field of table.

### Create Repository
> A repository acts as a meditator between the application and the underlying database
- It encapsulates the logic for querying, saving, updating, and deleting entities, allowing the application to interact with the database without dealing with low-level SQL or JDBC operations directly

To use repository in application, define interfact to inherit spring data repository interface like ``interface CoffeeRepository extends CrudRepository<Coffee, String>{}```
=> defined two type are type of object which would be stored and type of ID

- By extending these interfaces, you inherit the implementation of these methods, eliminating the need to write boilerplate code

<hr>

## 3. Refactoring
- Seperate API logic and data loading logic with Data loader class

```java
@Component
class DataLoader
{
    private final CoffeeRepository coffeeRepository;
    public DataLoader(CoffeeRepository coffeeRepository)
    {
        this.coffeeRepository = coffeeRepository;
    }

    @PostConstruct
    private void loadData()
    {
        coffeeRepository.saveAll(List.of(
                new Coffee("Cafe Cereza"),
                new Coffee("Cafe Ganador"),
                new Coffee("Cafe Lareno"),
                new Coffee("Cafe Tres Pontas")
        ));
    }
}
```

> '**@PostConstruct**' is an annotation used to mark a method that should be executed after a bean has been constructed and all dependencies have been injected