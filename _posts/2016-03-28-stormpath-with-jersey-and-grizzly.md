---
layout: post
title: Stormpath with Jersey and Grizzly
---

Oh, let's just use [Grizzly](https://grizzly.java.net), I thought.  It'll be so simple.  All the [Jersey](https://jersey.java.net) docs includes examples for it.

Yeah...  No.

My [initial problem]({% post_url 2016-03-21-grizzly-and-jersey-with-custom-application %}) with getting the two to work together should have been a red flag, but I wasn't paying attention.  The problem is there's just too much documentation out there on older versions of Jersey and Grizzly that it's difficult to find anything relevant on the current version.  It's not rocket science, but it's not exactly obvious for anyone new to Grizzly either.

This is how I finally got [Stormpath](https://stormpath.com) working in Grizzly with Jersey.

The main problem is that Grizzly can't be configured via `web.xml`.  Everything has to be done manually.

```java
/**
 * This is based on {@link GrizzlyWebContainerFactory#create(String)}.
 */
public HttpServer initializeServer() {
  URI uri = URI.create(m_baseUri);
  String path = uri.getPath();
  if (path == null) {
    throw new IllegalArgumentException("The URI path, of the URI " + uri + ", must be non-null");
  } else if (path.isEmpty()) {
    throw new IllegalArgumentException("The URI path, of the URI " + uri + ", must be present");
  } else if (path.charAt(0) != '/') {
    throw new IllegalArgumentException("The URI path, of the URI " + uri + ". must start with a '/'");
  }
  path = String.format("/%s", UriComponent.decodePath(uri.getPath(), true).get(1).toString());

  WebappContext context = new WebappContext("GrizzlyContext", path);
  ServletRegistration registration;
    registration = context.addServlet(ServletContainer.class.getName(), ServletContainer.class);
  registration.addMapping("/*");
  
  // this enables your custom javax.ws.rs.Application
  registration.setInitParameter(ServletProperties.JAXRS_APPLICATION_CLASS,
      CustomApplication.class.getName());

  // this is how you register a filter
  FilterRegistration filterRegistration = context.addFilter("StormpathFilter", StormpathFilter.class);
  filterRegistration.addMappingForUrlPatterns(EnumSet.allOf(DispatcherType.class), "/*");
  
  // this is how you register a listener
  context.addListener("com.stormpath.sdk.servlet.config.DefaultConfigLoaderListener");
  context.addListener("com.stormpath.sdk.servlet.client.DefaultClientLoaderListener");
  context.addListener("com.stormpath.sdk.servlet.application.DefaultApplicationLoaderListener");

  HttpServer server = GrizzlyHttpServerFactory.createHttpServer(uri);
  context.deploy(server);
  return server;
}
```

One last gotcha: Grizzly won't be able to find `stormpath.properties` if it's in `/WEB-INF`.  It has to be in the classpath.
