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
