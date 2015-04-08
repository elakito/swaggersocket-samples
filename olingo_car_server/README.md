Apache Olingo OData SwaggerSocket enabled Cars service
=================================================

This demo illustrates how to develop a simple OData
service that can be invoked using SwaggerSocket. SwaggerSocket is
a protocol that allows any existing REST services to be invoked
over WebSocket. See further information on SwaggerSocket at its
project page at
[https://github.com/swagger-api/swagger-socket](https://github.com/swagger-api/swagger-socket)

Building
--------
From the base directory of this sample, the pom.xml file
is used to build and run the standalone unit test.

```bash
  mvn clean install
```

Running the demo
------------------------
The war file can be deployed to any servlet container that supports WebSocket.
Alternatively, you can use the embedded jetty using mvn jetty:run


```bash
mvn jetty:run
```

You can open http://localhost:8080/ to access the default page and see some available
queries.

To use SwaggerSocket to invoke these queries, open http://localhost:8080/index.html or
http://localhost:8080/index_fallback.html. The first one uses swaggersocket.js to invoke
the queries over WebSocket. The second one uses a plain javascript to invoke the queries
over WebSocket and displays the transcribed SwaggerSocket messages.

