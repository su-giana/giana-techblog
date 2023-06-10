---
title: "Spring Data"
---

> I referred Spring Boot Up & Running from O'REILLY. In this book, author utilized his(/her) own Spring boot RESTful API. 

## 1. Create template based service with Redis
 > Redis is an open-source, in-memory data structure store that can be used as a database, cache, and message broker

 ### 1.1. Initialize project
**options**
 - Maven
 - Java: 17
 - Spring boot: latest
 - Packaging: Jar

**dependency**
- spring-boot-starter-webflux
- spring-boot-starter-data-redis
- lombok

### 1.2. Assume we can receive JSON response same with below paragraph
```json
[
    {
        "id": 108,
        "callsign": "callsign",
        "squawk": "1111",
        "filghtno": "",
        "route": "ROUTE",
        "type": "TYPE",
        "category": "category",
        "altitude": 100000,
        "heading": 100,
        "speed": 100,
        "lat": 10,
        "lon": 10,
        "is_adsb": true,
        "is_on_ground": false,
        "last_seen_time": 2020-11-11,
        "pos_update_time": 2020-11-11,
        "bds40_seen_time": null,
    },
    {<another aircraft in range, same fields as above>},
    {<ifnal aircraft currently in range, smae fields as above>}
]
```

#### Define doamin class

```java
package com.example.demo2;

import com.fasterxml.jackson.annotation.JsonIgnoreProperties;
import com.fasterxml.jackson.annotation.JsonProperty;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.springframework.data.annotation.Id;

import java.time.Instant;

@Data
@NoArgsConstructor
@AllArgsConstructor
@JsonIgnoreProperties(ignoreUnknown = true)
public class AirCraft {
    @Id
    private Long id;
    private String callsign, sqawk, reg, flightno, route, type, category;
    private int altitude, heading, speed;
    @JsonProperty("vert_rate")
    private int vertRate;
    @JsonProperty("selected_altitude")
    private int selectedAltitude;
    private double lat, lon, barometer;
    @JsonProperty("polar_distance")
    private double polarDistance;
    @JsonProperty("polar_bearing")
    private double polarBearing;
    @JsonProperty("is_adsb")
    private boolean isADSB;
    @JsonProperty("is_on_ground")
    private boolean isOnGround;
    @JsonProperty("last_seen_time")
    private Instant lastSeenTime;
    @JsonProperty("pos_update_time")
    private Instant posUpdateTime;
    @JsonProperty("bs40_seen_time")
    private Instant bds40SeenTime;

    public String getLastSeenTime() {return lastSeenTime.toString();}

    public void setLastSeenTime(String lastSeenTime)
    {
        if(null != lastSeenTime)
        {
            this.lastSeenTime = Instant.parse(lastSeenTime);
        }
        else
        {
            this.lastSeenTime = Instant.ofEpochSecond(0);
        }
    }

    public String getPosUpdateTime()    {return posUpdateTime.toString();}

    public void setPosUpdateTime(String posUpdateTime)
    {
        if(null != posUpdateTime)
        {
            this.posUpdateTime = Instant.parse(posUpdateTime);
        }
        else
        {
            this.posUpdateTime = Instant.ofEpochSecond(0);
        }
    }

    public String getBds40SeenTime()    {return bds40SeenTime.toString();}

    public void setBds40SeenTime(String bds40SeenTime)
    {
        if(null!=bds40SeenTime)
        {
            this.bds40SeenTime = Instant.parse(bds40SeenTime);
        }
        else
        {
            this.bds40SeenTime = Instant.ofEpochSecond(0);
        }
    }
}
```

- @Data: Lombok makes data class using getter, setter, equals(), hashCode(), toString() methods
- @NoArgsConstructor: Lombok generates a default constructor with no arguments for a class
- @AllArgsConstructor: Lombok generates a constructor with arguments for all non-static fields of a class
- @JsonIgnoreProperties(ignoreUnknown = true): ignore any unknown properties during the deserialization process of JSON objects
- @JsonProperty("vert_rate"): map a Java class property to a specific JSON poperty during serialization and deserialziation

### Adding template support
```java
@SpringBootApplication
public class Demo2Application {

    @Bean
    public RedisOperations<String, AirCraft>
    redisOperations(RedisConnectionFactory factory)
    {
        Jackson2JsonRedisSerializer<AirCraft> serializer = new Jackson2JsonRedisSerializer<>(AirCraft.class);

        RedisTemplate<String, AirCraft> template = new RedisTemplate<>();
        template.setConnectionFactory(factory);
        template.setDefaultSerializer(serializer);
        template.setKeySerializer(new StringRedisSerializer());

        return template;
    }

    public static void main(String[] args) {
        SpringApplication.run(Demo2Application.class, args);
    }

}
```

### Unifying
Below code sending airplane record creating @Component class and polling PlaneFinder endpoint, handle Aircraft record received by Redis template support. Next, made method polling information regularly.
```java
@EnableScheduling
@Component
class PlanFinderPoller {
    private WebClient client = WebClient.create("http://localhost:7634/aircraft");

    private final RedisConnectionFactory connectionFactory;
    private final RedisOperations<String, AirCraft> redisOperations;

    PlanFinderPoller(RedisConnectionFactory connectionFactory, RedisOperations<String, AirCraft> redisOperations) {
        this.connectionFactory = connectionFactory;
        this.redisOperations = redisOperations;
    }

    @Scheduled(fixedRate = 1000) // polling information 1 time per second
    private void pollPlanes()
    {
        connectionFactory.getConnection().serverCommands().flushDb();   // connection to DB and flush all key

        client.get()
                .retrieve()
                .bodyToFlux(AirCraft.class)
                .filter(plane -> !plane.getReg().isEmpty()) // filter non-registered plane
                .toStream()
                .forEach(ac ->
                        redisOperations.opsForValue().set(ac.getReg(), ac));    // store to redis database

        redisOperations.opsForValue()
                .getOperations()
                .keys("*") // retrieve using all keys
                .forEach(ac ->      // print them
                        System.out.println(redisOperations.opsForValue().get(ac)));
    }
}
```

<hr>

## 2. Convert template to repository

1. Define repository extends CrudRepository
```java
public interface AircraftRepository extends CrudRepository<AirCraft, Long> {}
```

2. Switch RedisOperations to repostiroy and remove Bean

```java
@SpringBootApplication
public class Demo2Application {
    public static void main(String[] args) {
        SpringApplication.run(Demo2Application.class, args);
    }

}

@EnableScheduling
@Component
class PlanFinderPoller {
    private WebClient client = WebClient.create("http://localhost:7634/aircraft");

    private final RedisConnectionFactory connectionFactory;
    private final AircraftRepository repository;

    PlanFinderPoller(RedisConnectionFactory connectionFactory, AircraftRepository repository) {
        this.connectionFactory = connectionFactory;
        this.repository = repository;
    }

    @Scheduled(fixedRate = 1000) // polling information 1 time per second
    private void pollPlanes()
    {
        connectionFactory.getConnection().serverCommands().flushDb();   // connection to DB and flush all key

        client.get()
                .retrieve()
                .bodyToFlux(AirCraft.class)
                .filter(plane -> !plane.getReg().isEmpty()) // filter non-registered plane
                .toStream()
                .forEach(repository::save);    // store to redis database

        repository.findAll().forEach(System.out::println);
    }
}
```

3. Put @RedisHash annotation to AirCraft class to mark aggregate root

<hr>

## 3. Create repository based service using JPA

### 3.1. Initialize project
**Options**
- Maven
- Java: 17
- Packaging: Jar
**Dependencies**
- Spring reactive web
- Spring Data JPA
- MySQL Driver
- Lombok

### 3.2. Develop JPA(MySQL) Service

#### Define domain class
```java
package com.example.deom3;

import com.fasterxml.jackson.annotation.JsonProperty;
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.Id;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.time.Instant;

@Entity
@Data
@NoArgsConstructor
@AllArgsConstructor
public class AirCraft
 {
    @Id
    @GeneratedValue
    private Long id;
    private String callsign, sqawk, reg, flightno, route, type, category;
    private int altitude, heading, speed;

    @JsonProperty("vert_rate")
    private int vertRate;
    @JsonProperty("selected_altitude")
    private int selectedAltitude;
    private double lat, lon, barometer;
    @JsonProperty("polar_distance")
    private double polarDistance;
    @JsonProperty("polar_bearing")
    private double polarBearing;
    @JsonProperty("is_adsb")
    private boolean isADSB;
    @JsonProperty("is_on_ground")
    private boolean isOnGround;
    @JsonProperty("last_seen_time")
    private Instant lastSeenTime;
    @JsonProperty("pos_update_time")
    private Instant posUpdateTime;
    @JsonProperty("bs40_seen_time")
    private Instant bds40SeenTime;
}

public interface AircraftRepository extends CrudRepository<AirCraft, Long> {}

@EnableScheduling
@Component
@RequiredArgsConstructor
class PlaneFinderPoller
{
    @NonNull
    private final AircraftRepository repository;
    private WebClient client = WebClient.create("http://localhost:7634/aircraft");

    @Scheduled(fixedRate = 1000)
    private void pollPlanes()
    {
        repository.deleteAll();

        client.get()
                .retrieve()
                .bodyToFlux(AirCraft.class)
                .filter(plane -> !plane.getReg().isEmpty())
                .toStream()
                .forEach(repository::save);
        repository.findAll().forEach(System.out::println);
    }
}
```

### 3.3. Data Load
#### Create Database script
- schema-mysql.sql
```sql
DROP TABLE IF EXISTS aircraft;
CREATE TABLE aircraft (id BIGINT not null primary key, callsign VARCHAR(7), squawk VARCHAR(4), reg VARCHAR(6)
                      filghtno VARCHAR(10), route VARCHAR(25), type VARCHAR(4), category VARCHAR(2),
                      altitude INT, heading INT, speed INT, vert_rate INT, selected_altitude INT,
                      lat DOUBLE, lon DOUBLE, barometer DOUBLE,
                      polar_distance DOUBLE, polar_bearing DOUBLE,
                      isadsb BOOLEAN, is_on_ground BOOLEAN,
                      last_seen_time TIMESTAMP, pos_update_time TIMESTAMP, bds40seen_time TIMESTAMP);
```

- data-mysql.sql
```sql
INSERT INTO aircraft(id, callsign, squawk, reg, flightno, route, type, category, altitude, heading, speed,
                     vert_rate, selected_altitude, lat, lon, barometer, polar_distance, polar_bearing,
                     isadsb, in_on_ground, last_seen_time, pos_update_time, bds40seen_time)
VALUES(80, 'callsign', '1000', 'registered', 'N1000', 'route1', 'KOR-JPN', 'A100', 10000, 200, 300, 0, 3600,
       10, 10, 1000, 100, 100, true, false, '020-11-27 21:29:35', '2020-11-27 21:29:35');
```

- Revise **application.properties**
```
spring.sql.init.mode=always
spring.jpa.hibernate.ddl-auto=none
```
- 1st line: initialize data base when the application start to run
- 2nd line: Disable auto table creation function of spring boot

<hr>

## 4. Creating repository based service using NoSQL document database

### 4.1. Initialize project
**options**
- Gradle
- Kotlin
- Spring boot: latest
- Packaging: Jar
- Java: 17
**dependencies**
- spring reactive web
- spring data mongoDB
- mongoDB database

> Add below line in build.gradle.kts's dependency

```kotlin
implementation("de.flapdoodle.embed:de.flapdoodle.embed.mongo:4.5.1")
```

- The process is same with above

- PlaneFinderPoller.kts

```kotlin
package com.example.demo4

import org.springframework.scheduling.annotation.Scheduled
import org.springframework.web.reactive.function.client.WebClient
import org.springframework.web.reactive.function.client.bodyToFlux
import java.sql.DriverManager.println

class PlaneFinderPoller(private val repository: AircraftRepository) {
    private val client = WebClient.create("http://localhost:7634/aircraft")

    @Scheduled(fixedRate = 1000)
    private fun pollPlanes()
    {
        repository.deleteAll();

        client.get()
                .retrieve()
                .bodyToFlux<Aircraft>()
                .filter { !it.reg.isNullOrEmpty() }
                .toStream()
                .forEach{repository.save(it)}

        println("----- All Aircraft -----")
        repository.findAll().forEach{ println(it) }
    }
}
```

- Aircraft.kts

```kotlin
package com.example.demo4

import com.fasterxml.jackson.annotation.JsonIgnoreProperties
import com.fasterxml.jackson.annotation.JsonProperty
import org.springframework.data.annotation.Id
import org.springframework.data.mongodb.core.mapping.Document
import java.time.Instant

@Document
@JsonIgnoreProperties(ignoreUnknown = true)
data class Aircraft
(
        @Id val id: String,
        val callsign: String? = "",
        val squawk: String? = "",
        val reg: String? = "",
        val flightno: String? = "",
        val route: String? = "",
        val type: String? = "",
        val category: String? = "",
        val altitude: Int? = 0,
        val heading: Int? = 0,
        val speed: Int? = 0,
        @JsonProperty("vert_rate")  val verRate: Int? = 0,
        @JsonProperty("selected_altitude")
        val selectedAltitude: Int? = 0,
        val lat: Double? = 0.0,
        val lon: Double? = 0.0,
        val barometer: Double? = 0.0,
        @JsonProperty("polar_distance")
        val polarDistance: Double? = 0.0,
        @JsonProperty("polar_bearing")
        val polarBearing: Double? = 0.0,
        @JsonProperty("is_adsb")
        val isADSB: Boolean? = false,
        @JsonProperty("is_on_ground")
        val isOnGround: Boolean? = false,
        @JsonProperty("last_seen_time")
        val lastSeenTime: Instant? = Instant.ofEpochSecond(0),
        @JsonProperty("pos_update_time")
        val posUpdateTime: Instant? = Instant.ofEpochSecond(0),
        @JsonProperty("bds40_seen_time")
        val bds40SeenTime: Instant? = Instant.ofEpochSecond(0)
        )
```

- AircraftRepository.kts

```kotlin
package com.example.demo4

import org.springframework.data.repository.CrudRepository

interface AircraftRepository: CrudRepository<Aircraft, String>
```