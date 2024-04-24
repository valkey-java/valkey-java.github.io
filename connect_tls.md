---
title: TLS
layout: default
parent: How to connect
nav_order: 3
---

The following code shows how to use jackey to enable SSL and connect to valkey:
# Standalone
```java
import java.io.FileInputStream;
import java.io.InputStream;
import java.security.KeyStore;
import java.security.SecureRandom;
import javax.net.ssl.SSLContext;
import javax.net.ssl.SSLSocketFactory;
import javax.net.ssl.TrustManager;
import javax.net.ssl.TrustManagerFactory;

import org.apache.commons.pool2.impl.GenericObjectPoolConfig;

public class JackeySSLTest {
    private static SSLSocketFactory createTrustStoreSSLSocketFactory(String jksFile) throws Exception {
        KeyStore trustStore = KeyStore.getInstance("jks");
        InputStream inputStream = null;
        try {
            inputStream = new FileInputStream(jksFile);
            trustStore.load(inputStream, null);
        } finally {
            inputStream.close();
        }

        TrustManagerFactory trustManagerFactory = TrustManagerFactory.getInstance("PKIX");
        trustManagerFactory.init(trustStore);
        TrustManager[] trustManagers = trustManagerFactory.getTrustManagers();

        SSLContext sslContext = SSLContext.getInstance("TLS");
        sslContext.init(null, trustManagers, new SecureRandom());
        return sslContext.getSocketFactory();
    }

    public static void main(String[] args) throws Exception {
        // When you don't have a jks file, just set sslSocketFactory to null.
        final SSLSocketFactory sslSocketFactory = createTrustStoreSSLSocketFactory(<your_jks_file_path>);
        io.jackey.JedisPool jedisPool = new io.jackey.JedisPool(new GenericObjectPoolConfig(), <host>,
            <port>, <timeout>, <password>, 0, true, sslSocketFactory, null, null);

        try (io.jackey.Jedis jedis = pool.getResource()) {
            jedis.set("key", "value");
            System.out.println(jedis.get("key"));
        } catch (Exception e) {
            e.printStackTrace();
        }

        jedisPool.close(); // when app exit, close the resource.
    }
}
```

# Cluster
