---
layout: post
title: 基于Spring Cloud系的微服务架构! - Part01
categories: [arch]
tags: [arch, spring, spring-boot, microservices]
fullview: true
comments: true
---
## 00.顺义，北京

前两年一写方案就是SOA，经历了从一开始觉得SOA真牛逼，到后面SOA就是SHI，就是吹牛，就是忽悠...

去年开始微服务突然就出来了，16年新年一过，各种讨论突然就多了，竟然还有专门的微服务的论坛和专题研讨会了....

虽然我还么有机会了解到一个成功的微服务改造案例，但是也跟风搞一下吧。

其实主要还是去年有一次选择，在大数据和云架构上，选择了云架构，把spring cloud系，netflix系的东西都搞了搞，虽然么出大的成果，但学习了不少东西。

在云架构的支撑上，微服务，或者说重新命名的SOA，个人觉得这次能够持续挺长一段时间，没准就颠覆了某些东西。

虽然很多人不看好微服务，各种观点吧，作为个人兴趣出发，打算记录一下用Spring Cloud来实现的微服务思想。

不多说，先借张图总结下我的基本观点

![mono vs micro](http://7xr4lt.com1.z0.glb.clouddn.com/Monolithic-vs-Microservices-Future-Processing.jpg-wm "Shit World")

## 01.一些基本

### 0101.The 12 Factors

![The 12 Factors](http://7xr4lt.com1.z0.glb.clouddn.com/Selection_060.png-wm "12 monkeys")

[The Tweleve-Factor App官网](www.12factor.net)

内容不多说，方法论的东西，往往都是正确的,有用的，虽然一实施起来很容易就变样 :9，例如：设计模式... 但是思想是无价的，努力让自己变得更好吧

### 0102.Spring Cloud

![The Spring Tree](http://7xr4lt.com1.z0.glb.clouddn.com/spring-io-tree.png-wm "Spring IO Tree")

[Spring Cloud官网](http://projects.spring.io/spring-cloud/)

我承认我是Spring的big fan，从最初的framework，mvc，到现在的boot，cloud系列，我无条件优先选择spring，各种anti粉退散。

Spring Cloud里面包括了很多通用或者说核心的分布式解决方案和工具，这里面包括Config, Bus, Security, Stream, Cluster，Dis-sessions等等，OK，很多我都么用过..

Cloud系列里面还有个小组织叫Spring-Cloud-Netflix，里面是Netflix系的开源落地组件，拆包即用，包括了

- Service Discovery - Eureka
- Circuit Breaker - Hystrix
- REST declaration - Feign
- Client side load balance - Ribbon
- Routing & Filtering - Zuul

这次主要的核心就是用他们来搭个DEMO

### 0103.Spring Boot

![The Spring Boot](http://7xr4lt.com1.z0.glb.clouddn.com/spring-boot-logo.png-wm "Spring Boot")

[Spring Boot官网](http://projects.spring.io/spring-boot/)

要是软件行业也有Oscar，我绝对投Spring Boot，不解释..

没用过的，还在抨击Spring过重的，赶紧去试一下吧.

从一定程度上来说，Spring系列仔细读读就是微服务的最佳实践，而其中一个核心服务就是Spring Boot.

### 0104.Angular JS

此处有A图预警

![A图](http://7xr4lt.com1.z0.glb.clouddn.com/atu.jpg-wm "A")

我对前端技术基本就是 大部分HTML + 一部分JAVASCRIPT + 一点点CSS

所以对于AngularJS这种所谓的MV...VM，基本就是初学者水平，感觉写起来挺简单，因此旧拿他来子写个简单的web page好了.

不知道用的对不对，A图已送上，懂行的请绕行.

## 02.一点设计

正赶上NBA季后赛，就拿NBA来作demo了，目前逻辑很简单，很多个NBA队伍，每个队伍有很多队员。

因此设计两个数据服务，一个是Team服务，一个是Player服务，然后一个WEB服务，总要有个View展现下。

因为是个简单的DEMO，因此暂时就不考虑用数据库了，数据直接写代码里面啦，重要的是思想，思想，思想.


