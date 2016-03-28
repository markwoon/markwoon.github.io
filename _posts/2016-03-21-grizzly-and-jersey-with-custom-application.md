---
layout: post
title: Using Jersey + Grizzly with a Custom Application
categories: rtfm
---

Couldn't find any good examples of working with Jersey in Grizzly (`grizzly-http-servlet-server`) and a custom `org.glassfish.jersey.server.ResourceConfig` or `javax.ws.rs.core.Application`.

After much experimentation, I finally figured it out:

```java
Map<String, String> initParams = new HashMap<>();
initParams.put(ServletProperties.JAXRS_APPLICATION_CLASS, CustomApplication.class.getName());
HttpServer server = GrizzlyWebContainerFactory.create(URI.create(baseUri), initParams); 
```
