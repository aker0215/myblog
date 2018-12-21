---
layout: post
title: "jboss的发布目录下部署两个服务引起的异常以及处理"
date: 2018-12-20 11:24
comments: true
tags: 
	- 笔记
---

- **先贴解决办法**
在web.xml里配置一个唯一的'webAppRootKey' ，这样就不会冲突了。
名字随便叫, 最好是和工程名称相关的, 别再和别的工程冲突了就好
```
  <context-param>
    <param-name>webAppRootKey</param-name>
    <param-value>myapp-A.root</param-value>
  </context-param>
```

- **错误起因**
在一个jboss的deployment的服务部署目录下放了两个工程的war包, 启动抛异常
贴出一部分错误堆栈, 安全原因隐藏了三方件的版本信息和工程的包名称, 用A.war和B.war代替了: 

 ```
JBWEB000287: Exception sending context initialized event to listener instance of class org.springframework.web.util.Log4jConfigListener: 
        java.lang.IllegalStateException: Web app root system property already set to different value: 'webapp.root' 
       = [/opt/jbshome/appserver/cmsServer1/tmp/vfs/temp/tempf3f4b51b442896b0/A.war-c1ce03fbdd37dff6/] 
        instead of [/opt/jbshome/appserver/cmsServer1/tmp/vfs/temp/tempf3f4b51b442896b0/B.war-5d98fb790815fc0f/] 
        - Choose unique values for the 'webAppRootKey' context-param in your web.xml files!
        at org.springframework.web.util.WebUtils.setWebAppRootSystemProperty(WebUtils.java:161) 
        at org.springframework.web.util.Log4jWebConfigurer.initLogging(Log4jWebConfigurer.java:119) 
        at org.springframework.web.util.Log4jConfigListener.contextInitialized(Log4jConfigListener.java:49) 
        at org.apache.catalina.core.StandardContext.contextListenerStart(StandardContext.java:3339) 
        at org.apache.catalina.core.StandardContext.start(StandardContext.java:3780)
        at org.jboss.as.web.deployment.WebDeploymentService.doStart(WebDeploymentService.java:163) 
        at org.jboss.as.web.deployment.WebDeploymentService.access$000(WebDeploymentService.java:61) 
        at org.jboss.as.web.deployment.WebDeploymentService$1.run(WebDeploymentService.java:96) 
        at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:511)
        at java.util.concurrent.FutureTask.run(FutureTask.java:266) 
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142) 
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617) 
        at java.lang.Thread.run(Thread.java:745) [rt.jar:1.8.0_65]
        at org.jboss.threads.JBossThread.run(JBossThread.java:122)
```

- **根本原因**
看异常堆栈, Log4jWebConfigurer在初始化日志的时候抛的异常
日志自己已经说了很明白了
```
Web app root system property already set to different value: 'webapp.root' 
       = [/opt/jbshome/appserver/cmsServer1/tmp/vfs/temp/tempf3f4b51b442896b0/A.war-c1ce03fbdd37dff6/] 
        instead of [/opt/jbshome/appserver/cmsServer1/tmp/vfs/temp/tempf3f4b51b442896b0/B.war-5d98fb790815fc0f/] 
        - Choose unique values for the 'webAppRootKey' context-param in your web.xml files!
```

大意是webapp.root的值被A工程取了默认值, 导致和B工程产生了冲突
并且也提示了Choose unique values for the 'webAppRootKey' context-param in your web.xml files 
也就是说我们可以在web.xml增加一个webAppRootKey值的配置就可以解决问题

