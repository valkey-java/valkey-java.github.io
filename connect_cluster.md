---
title: Cluster
layout: default
parent: How to connect
nav_order: 2
---

The following code shows how to use jackey to connect to valkey in cluster mode:
```java
import java.util.HashSet;
import java.util.Set;
import io.jackey.HostAndPort;

public class JackeyClusterTest {
    private static final int DEFAULT_TIMEOUT = 2000;
    private static final int DEFAULT_REDIRECTIONS = 5;
    private static io.jackey.JedisCluster jc; // be static or singleton, thread safety.

    public static void main(String[] args) {
        io.jackey.ConnectionPoolConfig config = new io.jackey.ConnectionPoolConfig();
        // It is recommended that you set maxTotal = maxIdle = 2*minIdle for best performance
        // In cluster mode, please note that each business machine will contain up to maxTotal links,
        // and the total number of connections = maxTotal * number of machines
        config.setMaxTotal(32);
        config.setMaxIdle(32);
        config.setMinIdle(16);

        Set<HostAndPort> jedisClusterNode = new HashSet<HostAndPort>();
        jedisClusterNode.add(new HostAndPort(host, port));
        jc = new io.jackey.JedisCluster(jedisClusterNode, DEFAULT_TIMEOUT, DEFAULT_TIMEOUT, DEFAULT_REDIRECTIONS,
            password, null, config);

        jc.set("key", "value"); // Note that there is no need to call jc.close() here, 
                                // the connection recycling is actively completed internally.
        System.out.println(jc.get("key"));

        jc.close(); // when app exit, close the resource.
    }
}
```