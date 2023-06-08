---
title: "HTTP Request of Spring"
---

```java
@RestController
@RequestMapping("/coffees")
class RestApiDemoController {
    private List<Coffee> coffees = new ArrayList<>();

    public RestApiDemoController() {
        coffees.addAll(List.of(
                new Coffee("Cafe Cereza"),
                new Coffee("Cafe Ganador"),
                new Coffee("Cafe Lareno"),
                new Coffee("Cafe Tres Pontas")
        ));
    }

    @GetMapping
    Iterable<Coffee> getCoffees() {
        return coffees;
    }

    @GetMapping("/{id}")
    Optional<Coffee>    getCoffeeById(@PathVariable String id)
    {
        for(Coffee c : coffees)
        {
            if(c.getId().equals(id))
            {
                return Optional.of(c);
            }
        }

        return Optional.empty();
    }

    @PostMapping
    Coffee postCoffee(@RequestBody Coffee coffee) {
        coffees.add(coffee);
        return coffee;
    }

    @PutMapping("/{id}")
    ResponseEntity<Coffee> putCoffee(@PathVariable String id, @RequestBody Coffee coffee) {
        int coffeeIndex = -1;

        for (Coffee c : coffees) {
            if (c.getId().equals(id)) {
                coffeeIndex = coffees.indexOf(c);
                coffees.set(coffeeIndex, coffee);
            }
        }

        return (coffeeIndex == -1) ?
                new ResponseEntity<>(postCoffee(coffee), HttpStatus.CREATED):
                new ResponseEntity<>(coffee, HttpStatus.OK);
    }

    @DeleteMapping("/{id}")
    void deleteCoffee(@PathVariable String id)
    {
        coffees.removeIf(c -> c.getId().equals(id));
    }
}
```

### 1. @RestControlller
- @RestController is an annotation in Spring that conbines the functionalities of @Controller and @ResponseBody
- It is used to create RESTful web services in Spring MVC, where the methods in a class annotated with @RestController are automatically mapped to specific HTTP requests and the reponse is serialized directly to the HTTP response body

### 2. Defined request mapping
Annotated the controller method with '**@RequestMapping**' or '**@GetMapping**', '**@PostMapping**', '**@PutMapping**' specifies the URL path and HTTP method that the method should handle

### 3. Returned Response
We used various annotation such as '**@ResponseBody**', '**@RestController**', '**String**', and '**ResponseEntity**'