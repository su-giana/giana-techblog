---
title: Spring MVC
---

> Spring MVC, which stands for Spring Model-View-Controller, is a framework for building web applications in Java

## 1. Interact with user using template engine

### 1.1. Initialize project

**options**
- Maven
- Java: 17
- Spring boot
- Packging: Jar

**dependencies**
- Spring Web
- Spring Reactive Web
- Thymeleaf
- Spring Data JPA
- H2 Database
- Lombok

> I used aladin open API (Korean bookstore website) to practice this session. Due to API policy issue, this section does not show the whole code.

### 1.2. Define domain class

```java
@Entity
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Book
{
    @Id
    private String isbn;

    private String title;

    private String link;

    private String authors;

    private String publishedDate;

    private String des;

    private String coverLink;

    private int price;
}
```

### Define Controller

```java
package com.example.devbookstore;

import com.fasterxml.jackson.databind.JsonNode;
import lombok.NonNull;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.reactive.function.client.WebClient;
import reactor.core.publisher.Mono;

@RequiredArgsConstructor
@Controller
public class BookController
{
    @NonNull
    private final BookRepository repository;
    private WebClient client =
            WebClient.create("http://www.aladin.co.kr/ttb/api/ItemList.aspx?ttbkey=ttbnsj67611232001&QueryType=ItemNewAll&MaxResults=10&start=1&SearchTarget=Book&output=JS&Version=20131101&CategoryId=351");

    @GetMapping("/books")
    public String getCurrentITBestSeller(Model model)
    {
        client.get()
                .retrieve()
                .bodyToFlux(JsonNode.class)
                // extract data of "item" section
                .flatMapIterable(jsonNode -> jsonNode.get("item"))
                // map bunch of data to object
                .map(this::parseBook)
                .toStream()
                .forEach(repository::save);

        model.addAttribute("bestSellers", repository.findAll());
        return "bookList.html";
    }

    // process nested json
    private Book parseBook(JsonNode bookNode)
    {
        Book book = new Book();
        book.setIsbn(bookNode.get("isbn").asText());
        book.setTitle(bookNode.get("title").asText());
        book.setAuthors(bookNode.get("author").asText());
        book.setPublishedDate(bookNode.get("pubDate").asText());
        book.setDes(bookNode.get("description").asText());
        book.setCoverLink(bookNode.get("cover").asText());
        book.setPrice(bookNode.get("priceStandard").asInt());

        return book;
    }
}
```

### Create View file

> You should place the file in src>resource>template directory

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8" http-equiv="Content-Type" content="text/html" />
    <title>bookList</title>
</head>
<body>

<h2>BestSellers</h2>

<div class="bestSellerList" th:unless="${#lists.isEmpty(bestSellers)}">
    <div th:each="book : ${bestSellers}">
        <img th:src="${book.coverLink}" width="50vw" height="30vw">
        <div>
            <p th:text="${book.title}"></p>
        </div>
    </div>
</div>

</body>
</html>
```

1. Identify Thymeleaf-specific attributes and expressions within HTML or XML template using "**th**" prefix
=> ```<html lang="en" xmlns:th="http://www.thymeleaf.org">```

2. Skip displaying some section when repository is empty

=> ```<div th:unless="${#lists.isEmpty(bestSellers)}">```

3. Fill data in View using each statement of thymeleaf

=> ```<tr th:each="ac : ${bestSellers}">```

4. Refer data with attribute of td

=> ```<td th:text="${ac.callsign}"></td>```

### Develop refreshing

```html
<script type="text/javascript">
    window.onload = setupRefresh;

    function setupRefresh()
    {
        setTimeout("refreshPage();", 5000); // interval == 5s
    }
    function refreshPage()
    {
        window.location = location.href;
    }
</script>
```

## 3. Messaging
> Messaging systems like Kafka and RabitMQ enable asynchronous communication, decoupling services, improving scalability, ensuring reliability, supporting event-driven architectures, and facilitating data integration and stream processing in distributed systems

Part three appends messaging system like Kafka and RabbitMQ for scalability and communication between applciations

### 3.1. Add dependency

```xml
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Finchley.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-amqp</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-stream</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-stream-binder-kafka</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-stream-binder-rabbit</artifactId>
        </dependency>
        <dependency>
```

To deliever bookstore application's information to library application, book information application would provide the state of book to library application.

