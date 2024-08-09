---
title: Introduction
layout: default
nav_order: 1
---

Valkey-java is [Valkey](https://github.com/valkey-io/valkey)'s Java client, derived from [Jedis](https://github.com/redis/jedis) fork, dedicated to maintaining simplicity and high performance.

# Getting started
Add the following dependencies to your `pom.xml` file, you can find the latest version of valkey-java at [Maven Central](https://central.sonatype.com/artifact/io.valkey/valkey-java).
```
<dependency>
    <groupId>io.valkey</groupId>
    <artifactId>valkey-java</artifactId>
    <version>5.3.0</version>
</dependency>
```

## Connect to Valkey
```java
public class ValkeyTest {
    // can be static or singleton, thread safety.
    private static io.valkey.JedisPool jedisPool;
    
    public static void main(String[] args) {
        io.valkey.JedisPoolConfig config = new io.valkey.JedisPoolConfig();
        // It is recommended that you set maxTotal = maxIdle = 2*minIdle for best performance
        config.setMaxTotal(32);
        config.setMaxIdle(32);
        config.setMinIdle(16);
        jedisPool = new io.valkey.JedisPool(config, <host>, <port>, <timeout>, <password>);
        try (io.valkey.Jedis jedis = jedisPool.getResource()) {
            jedis.set("key", "value");
            System.out.println(jedis.get("key"));
        } catch (Exception e) {
            e.printStackTrace();
        }
        jedisPool.close(); // when app exit, close the resource.
    }
}
```