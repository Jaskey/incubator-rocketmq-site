---
title: "Motivation"
permalink: /docs/motivation/
modified: 2016-12-16T15:01:43-04:00
---

At early stages, we constructed our distributed messaging middleware based on ActiveMQ 5.x(prior to 5.3). Our 
multinational business uses it for asynchronous communication, search, social network activity stream, data pipeline,
even in its trade processes. As our trade business throughput rises, pressure originating from our messaging cluster
also becomes urgent.

{% include toc %}

# Why RocketMQ ?

Based on our research, with increased queues and virtual topics in use, ActiveMQ IO module reaches a bottleneck. We 
tried our best to solve this problem through throttling, circuit breaker or degradation, but it did not work well. So 
we begin to focus on the popular messaging solution Kafka at that time. Unfortunately, Kafka can not meet our 
requirements especially in terms of low latency and high reliability, see [here](/rocketmq/how-to-support-more-queues-in-rocketmq/) for details.

In this context, we decided to invent a new messaging engine to handle a broader set of use cases, ranging from 
traditional pub/sub scenarios to high volume real-time zero-loss tolerance transaction system. We believe this solution
can be beneficial, so we would like to open source it to the community. Today, more than 100 companies are using the 
open source version of RocketMQ in their business. We also published a commercial distribution based on RocketMQ, a PaaS
 product called the [Alibaba Cloud Platform](https://intl.aliyun.com/).


The following table demonstrates the comparison between RocketMQ, ActiveMQ and Kafka (Apache's most popular messaging solutions according to [awesome-java](https://github.com/akullpp/awesome-java)):

# RocketMQ vs. ActiveMQ vs. Kafka


| Messaging Product|Client SDK| Protocol and Specification | Order Message  |Message Filter|Server Triggered Redelivery|Persistent Message|Retroactive Consumers|Message Priority|High Availability and Failover|Message Track|Configuration|Management and Operation Tools|
| -------|--------|--------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| ActiveMQ|Java, .NET, C++ etc. |Push model, support OpenWire, STOMP, AMQP, MQTT, JMS|Exclusive Consumer or Exclusive Queues can ensure ordering|Supported|Not Supported|Supports very fast persistence using JDBC along with a high performance journal，such as levelDB, kahaDB|Supported|Supported|Supported, depending on storage,if using kahadb it requires a ZooKeeper server|Not Supported|The default configuration is low level, user need to optimize the configuration parameters|Supported|
| Kafka      | Java, Scala etc.|Pull model, support TCP|Ensure ordering of messages within a partition|Supported, you can use Kafka Streams to filter messages|Not Supported|High performance file storage|Supported offset indicate|Not Supported|Supported, requires a ZooKeeper server|Not Supported|Kafka uses key-value pairs format for configuration. These values can be supplied either from a file or programmatically.|Supported, use terminal command to expose core metrics|
| RocketMQ      |Java, .NET, C++ |Pull model, support TCP, JMS|Ensure strict ordering of messages, have no hot spot problem,and can scale out gracefully|Supported, you can even upload yourself custom-built filter code snippets|Supported|High performance and low latency file storage|Supported timestamp and offset 2 indicates|Not Supported|Supported, Master-Slave model, without another kit|Supported|Work out of box,user only need to pay attention to a few configurations|Supported, rich web and terminal command to expose core metrics|