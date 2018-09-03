---
layout: post
title: Error05 java.lang.AbstractMethodError: org.slf4j.impl.JDK14LoggerAdapter.log
category: Errors 
tags: [slf4j, logger, error]
---


# 에러 

```
심각: Exception sending context initialized event to listener instance of class [org.springframework.web.context.ContextLoaderListener]
java.lang.AbstractMethodError: org.slf4j.impl.JDK14LoggerAdapter.log(Lorg/slf4j/Marker;Ljava/lang/String;ILjava/lang/String;[Ljava/lang/Object;Ljava/lang/Throwable;)V
    at org.apache.commons.logging.impl.SLF4JLocationAwareLog.info(SLF4JLocationAwareLog.java:155)
    at org.springframework.web.context.ContextLoader.initWebApplicationContext(ContextLoader.java:270)
    at org.springframework.web.context.ContextLoaderListener.contextInitialized(ContextLoaderListener.java:103)
    at org.apache.catalina.core.StandardContext.listenerStart(StandardContext.java:4637)
    at org.apache.catalina.core.StandardContext.startInternal(StandardContext.java:5099)
    at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:183)
    at org.apache.catalina.core.ContainerBase$StartChild.call(ContainerBase.java:1425)
    at org.apache.catalina.core.ContainerBase$StartChild.call(ContainerBase.java:1415)
    at java.util.concurrent.FutureTask.run(Unknown Source)
    at org.apache.tomcat.util.threads.InlineExecutorService.execute(InlineExecutorService.java:75)
    at java.util.concurrent.AbstractExecutorService.submit(Unknown Source)
    at org.apache.catalina.core.ContainerBase.startInternal(ContainerBase.java:941)
    at org.apache.catalina.core.StandardHost.startInternal(StandardHost.java:839)
    at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:183)
    at org.apache.catalina.core.ContainerBase$StartChild.call(ContainerBase.java:1425)
    at org.apache.catalina.core.ContainerBase$StartChild.call(ContainerBase.java:1415)
    at java.util.concurrent.FutureTask.run(Unknown Source)
    at org.apache.tomcat.util.threads.InlineExecutorService.execute(InlineExecutorService.java:75)
    at java.util.concurrent.AbstractExecutorService.submit(Unknown Source)
    at org.apache.catalina.core.ContainerBase.startInternal(ContainerBase.java:941)
    at org.apache.catalina.core.StandardEngine.startInternal(StandardEngine.java:258)
    at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:183)
    at org.apache.catalina.core.StandardService.startInternal(StandardService.java:422)
    at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:183)
    at org.apache.catalina.core.StandardServer.startInternal(StandardServer.java:770)
    at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:183)
    at org.apache.catalina.startup.Catalina.start(Catalina.java:682)
    at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
    at sun.reflect.NativeMethodAccessorImpl.invoke(Unknown Source)
    at sun.reflect.DelegatingMethodAccessorImpl.invoke(Unknown Source)
    at java.lang.reflect.Method.invoke(Unknown Source)
    at org.apache.catalina.startup.Bootstrap.start(Bootstrap.java:353)
    at org.apache.catalina.startup.Bootstrap.main(Bootstrap.java:493)
```

# 해결

pom.xml에 다음을 추가하기

```
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-jdk14</artifactId>
    <version>1.7.5</version>
</dependency>
```

# 원인

slf4j 버전 두개가 충돌. 그러나 라이브러리와 pom.xml을 다 봐도 충돌하는 버전이 없었음. 그래서 검색 후 위의 것을 추가했더니 해결
<br/>
maven update를 꼭하고 project clean, server clean을 해주는 것도 도움이 된다. 
<br/>
아래와 같이 추가할 수도 있다

```
<!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-jdk14 -->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-jdk14</artifactId>
            <version>${org.slf4j-version}</version>
            <scope>runtime</scope>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-nop -->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>${org.slf4j-version}</version>
            <scope>runtime</scope>
        </dependency>
```



> 출처 : [중국 웹](https://translate.google.co.kr/translate?hl=ko&sl=zh-CN&u=http://lcl088005.iteye.com/blog/2066215&prev=search)

