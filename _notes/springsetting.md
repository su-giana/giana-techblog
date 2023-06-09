---
title: "Setting and Management of Spring"
---

- From [[spring]]

## 1. Application Setting

### @ConfigurationProperties
> the annotation is used to bind external configuration properties to fields or properties of a Java class

1. By annotating a class with '**@ConfigurationProperties**', map external configuration properties to the fields or properties of that class
2. The configuration properties can be sourced from various locations, such as property files (e.g., '**application.properties**' or '**application.yml**')
3. Annotate the configuration class with '**@ConfigurationPropertiesScan**' and specify the classes annotated with '**ConfigurationProperties**'

```java
@ConfigurationProperties(prefix = "greeting")
class Greeting
{
    private String name;
    private String coffee;

    public String getName()
    {
        return name;
    }

    public void setName(String name)
    {
        this.name = name;
    }

    public String getCoffee()
    {
        return coffee;
    }

    public void setCoffee(String coffee)
    {
        this.coffee = coffee;
    }
}

@RestController
@RequestMapping("/greeting")
class GreetingController
{
    private final Greeting greeting;

    public GreetingController(Greeting greeting)
    {
        this.greeting = greeting;
    }

    @GetMapping
    String getGreeting()
    {
        return greeting.getName();
    }

    @GetMapping("/coffee")
    String getNameAndCoffee()
    {
        return greeting.getCoffee();
    }
}
```

### Third-party configuration
> The annotation is often used when configuring and intergrating third-party components within a Spring application
=> Create a Spring bean that represents the component and inject the relevant configuration properties

```java
@SpringBootApplication
@ConfigurationPropertiesScan
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }

    @Bean
    @ConfigurationProperties(prefix = "droid")
    Droid createDroid() {return new Droid();}
}

class Droid {
    private String id, description;

    public String getId()   {return id;}
    public void setId(String id)    {this.id = id;}
    public String getDescription()  {return description;}
    public void setDescription(String description)   {this.description = description;}
}

@RestController
@RequestMapping("/droid")
class DroidController
{
    private final Droid droid;

    public DroidController(Droid droid)
    {
        this.droid = droid;
    }

    @GetMapping
    Droid getDroid()    {return droid;}
}
```

## 2. Autoconfiguration Report
> Autoconfiguration Report is a helpful feature that provides information about the auto-configuration decisions made by the framework during application startup
- The report categorizes auto-configurations into "Positive Matches" and "Exclusion"
- Positive Matches : the auto-configurations that were applied based on the conditions and requirements of your application
- Exclusion : the auto-configurations that were skipped due to specific conditions not begin met or because you explicitly excluded them

## 3. Actuator
> Spring Actuator is a module in the Spring framework that provides production-ready features to monitor and manage Spring application
- It exposes a set of pre-defined HTTP endpoints that provide insight into application health, metrics, and other useful information

```xml
<dependency>
    <groupdId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

- You can choose entry points set you want as adding following line in **application.properties**
```
management.endpoints.web.exposure.include={preferred section}
```
=> Wildcard not recommended due to security

- For more detailed information related to health, put following line in **application.properties**
```
management.endpoint.health.how-details=always
```
- If you want to increase logging to help diagnose and resolve issues
```
echo '{"configuredLvel": "TRACE:}'
| http :8080/actuator/loggers/org.springframework.data.web
```