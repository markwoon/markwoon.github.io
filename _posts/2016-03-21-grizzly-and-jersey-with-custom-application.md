---
layout: post
title: Using Jersey + Grizzly with a Custom Application
categories: rtfm
---

Can't find any good examples of working with a custom `ResourceConfig` or `Application` from [Jersey](https://jersey.java.net) in [Grizzly](https://grizzly.java.net)'s `grizzly-http-servlet-server`.

After much experimentation, I finally figured it out:

```java
Map<String, String> initParams = new HashMap<>();
initParams.put(ServletProperties.JAXRS_APPLICATION_CLASS, CustomApplication.class.getName());
HttpServer server = GrizzlyWebContainerFactory.create(URI.create(baseUri), initParams); 
```
