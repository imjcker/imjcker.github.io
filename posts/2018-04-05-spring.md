---
layout: post
title: Spring All in One
category: spring-cloud
tags: spring
---

# 2018-04-05-spring

Spring 全家桶记录，主要是：spring boot、spring cloud以及相关配置。

## Spring Basics

**乱码**

spring 提供了过滤器解决post请求乱码问题. org.springframework.web.filter.CharacterEncodingFilter

## 动态注入bean

ImportBeanDefinitionRegistrar: 实现接口。

Configuration: spring boot 配置类

Import: 导入实现了ImportBeanDefinitionRegistrar 接口的类

## xxxAware

实现接口，像实现类注入xxx的属性值。

## Spring cloud

1. zuul 配置和优化

   zuul 配置和优化

2. ribbon 配置和优化

   ribbon

3. hystrix 配置

   **hystrix默认的隔离策略是thread，但是在zuul中，默认的hystrix隔离策略是semaphore**

   这个超时时间要根据所对应的业务和服务器所能承受的负载来设置，要根据业务的平均响应时间设置，一般是大于平均响应时间的20%~100%,最好是根据压力测试结果来评估，这个值设置太大，会导致线程不够用而会导致太多的任务被fallback；设置太小，一些特殊的慢业务失败率提升，甚至会造成这个业务一直无法成功，在重试机制存在的情况下，反而会加重后端服务压力。

4. ribbon超时 = \(ribbonReadTimeout + ribbonConnectTimeout\)  _\(maxAutoRetries + 1\)_  \(maxAutoRetriesNextServer + 1\);（发现：重试失败之后，熔断超时之前，也会熔断）
5. zuul中

   zuul开启重试，ribbon的重试才会起效；feign开启hystrix功能，且hystrix的timeout.enabled为false时熔断超时为ribbon的ReadTimeout\*\(retries + 1\)\(nextServer + 1\)

   ```yaml
   zuul:
       retryable: true
   ribbon:
       ConnectTimeout: 10000
       ReadTimeout: 30000
       MaxAutoRetries: 1
       MaxAutoRetriesNextServer: 1
       OkToRetryOnAllOperrations: true
   feign:
       hystrix:
           enabled: true
   hystrix:
       command:
           default:
               execution:
                   timeout:
                       enabled: false
                   isolation:
                       thread:
                           timeoutInMilliseconds: 最后优先级的hystrix超时配置(ribbon的超时起效)
   ```

6. Gateway 配置

   \`\`\`yaml spring: application: name: spring-cloud-service-gateway cloud: gateway: routes:

   * id: spring-cloud-service-wechat

     uri: lb://spring-cloud-service-wechat

     predicates:

     * Path=/wechat/\*\*

   * id: spring-cloud-service-manager

     uri: lb://spring-cloud-service-manager

     predicates:

     * Path=/manager/\*\*

   * id: spring-cloud-service-mail

     uri: lb://spring-cloud-service-mail

     predicates:

     * Path=/mail/\*\*

   * id: spring-cloud-service-blog

     uri: lb://spring-cloud-service-blog

     predicates:

     * Path=/blog/\*\*

\`\`\`

