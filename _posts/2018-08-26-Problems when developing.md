---
layout: post
title:  "Problems when developing"
date:   2018-08-26
excerpt: "Problems when developing"
tag:
- problems
comments: true
---

## List of Problems
### eclipse中SLF4J报错 "Failed to instantiate SLF4J LoggerFactor"
分析：eclipse无法从maven的本地仓库中加载到SLF4J的依赖JAR包，尽管在eclipse的maven dependencies中可以看到SLF4J的依赖jar包，即仓库中已经下载了依赖，但是项目无法加载。

Solved:

Run： 

(1)mvn dependency:purge-local-repository

(2)mvn clean verify

### NCE存量同步

NCE存量同步使用到kafka消息队列（RHMessage）：Producer将存量变更（add、delete、update）的topic放到kafka（MessagingLBService）中，Consumer从kafka中根据相应的接口（以RESTful的API）去获取到kafka中的topic信息，然后对topic（message）进行解析，即是对存量的变更信息读取出来，写入存量同步的文件中，然后后续其他存量同步的微服务通过读取这个存量同步的文件进行更新同步存量信息。

存量同步kafka消息队列业务中的改进：后续对Producer入队的topic进行解析，发现其插入的若干topic有些具有很大的相似性，因此将几十个topic合并成几个topic，Consumer只需要从kafka消费（出队）少量的topic即可，但是合并后的topic里面的message有些在该微服务是不需要处理的，同时也对不需要处理的message进行过滤（根据message的className或者relationName进行过滤），避免消费了不必处理的message。
