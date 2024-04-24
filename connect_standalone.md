---
title: Standalone
layout: default
parent: How to connect
nav_order: 1
---

The following code shows how to use jackey to connect to valkey in standalone mode:
```java
public class JackeyTest {
    // can be static or singleton, thread safety.
    private static io.jackey.JedisPool jedisPool;
    
    public static void main(String[] args) {
        io.jackey.JedisPoolConfig config = new io.jackey.JedisPoolConfig();
        // It is recommended that you set maxTotal = maxIdle = 2*minIdle for best performance
        config.setMaxTotal(32);
        config.setMaxIdle(32);
        config.setMinIdle(16);
        jedisPool = new io.jackey.JedisPool(config, <host>, <port>, <timeout>, <password>);
        try (io.jackey.Jedis jedis = jedisPool.getResource()) {
            jedis.set("key", "value");
            System.out.println(jedis.get("key"));
        } catch (Exception e) {
            e.printStackTrace();
        }
        jedisPool.close(); // when app exit, close the resource.
    }
}
```