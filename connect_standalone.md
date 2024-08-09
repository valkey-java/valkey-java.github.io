---
title: Standalone
layout: default
parent: How to connect
nav_order: 1
---

The following code shows how to use valkey-java to connect to valkey in standalone mode:
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