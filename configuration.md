---
title: Configuration
layout: default
nav_order: 3
---

The following are the common parameters of apache common-pool and their meanings：

| Parameter | Meanings                                                                                                                                                                                                         | Default value | Recommended value              |
| --- |------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|--------------------------------|
|connectionTimeout| Initialize the timeout period for connecting to the cluster, such as the timeout period for reconnecting the cluster at startup and after the TCP connect is disconnected.                                       | 2000          | 5000                           |
|soTimeout| The timeout period for API access. For example, the timeout period for operations such as set and get.                                                                                                           | 2000          | 2000                           |
|maxTotal/maxIdle/minIdle | standalone mode: the connection to redis; cluster mode: The number of connections to a node in the cluster                                                                                                       | 8，8，0         | MaxTotal = MaxIdle = 2*MinIdle |
|blockWhenExhausted| When the resource pool is used up, whether the caller needs to wait or not. If not, an exception with insufficient connection is returned. The following maxWaitMillis takes effect only when the value is true. | true          | true                           |
|maxWaitMillis| The maximum wait time (in milliseconds) of the caller when the resource pool connection is exhausted.                                                                                                            | -1            | depending on your business     |
|testOnBorrow| Whether to check the validity of the connection (send the ping command) when borrowing the connection from the resource pool. The detected invalid connection will be removed.                                   | false         | false                          |
|testOnReturn| Whether to check the validity of the connection (send a ping command) when returning the connection to the resource pool. The detected invalid connection will be removed.                                       | false         | false                          |
|testOnCreate| If you create a new connection when borrowing a connection, we recommend that you disable it if you check whether the connection validity is performed (send a ping command).                                    | false         | false                          |
|testWhileIdle| Whether to check the validity of the connection (send a ping command) when detecting idle connections. If the connection is invalid, it will be closed.                                                          | true          | true                           |
|timeBetweenEvictionRunsMillis| The detection period of idle resources. Unit: milliseconds.                                                                                                                                                      | 30000         | 30000                          |
|minEvictableIdleTimeMillis| The minimum idle time (in milliseconds) of resources in the resource pool. When this value is reached, idle resources are removed. Unit: milliseconds.| 60000         | 60000                          |
|numTestsPerEvictionRun|The number of resources that are detected each time when idle resources are detected.| -1            | -1                             |
|evictionPolicy|Set the evict class, including the elimination algorithm. The default implementation is DefaultEvictionPolicy, which is eliminated according to the idle time.|         DefaultEvictionPolicy      |     DefaultEvictionPolicy                           |
|evictionPolicyClassName|Set the evict class name. The default implementation is DefaultEvictionPolicy, which is eliminated according to the idle time.|      DefaultEvictionPolicy         |         DefaultEvictionPolicy                       |
|evictorShutdownTimeoutMillis|The default waiting time when you exit the evictor.Unit: milliseconds.    |       10000        |                  10000              |
|fairness|When the connection pool is exhausted, multiple threads may block waiting for resources. If the fairness is true, threads can obtain resources in sequence.|        false       |           false                     |
|lifo|When multiple connections are available in the connection pool, a connection is selected based on this value. (Last in, First out)|         true      |        true                        |