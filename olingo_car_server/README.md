Apache Olingo OData SwaggerSocket enabled Cars service
=================================================

This demo illustrates how to develop a simple OData
service that can be invoked using SwaggerSocket. SwaggerSocket is
a protocol that allows any existing REST services to be invoked
over WebSocket. See further information on SwaggerSocket at its
project page at
[https://github.com/swagger-api/swagger-socket](https://github.com/swagger-api/swagger-socket)

This demo uses the Cars service sample from Apache Olingo project, which is available at [https://github.com/apache/olingo-odata4/tree/master/samples/server](https://github.com/apache/olingo-odata4/tree/master/samples/server).

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

#### Testing this demo

Using Browser, open [http://localhost:8080/](http://localhost:8080/) to access the default page and see some available
queries. You can invoke these queries over HTTP by clicking on those links or directly typing
in the queries in Browser's URL field.

To use SwaggerSocket to invoke these queries, open [http://localhost:8080/index.html](http://localhost:8080/index.html).

The page uses swaggersocket.js to connect to the server and invoke
the queries asynchronously over a single WebSocket. To view which SwaggerSocket messages are sent and received, you can go to
the fallback page at [http://localhost:8080/index_fallback.html](http://localhost:8080/index_fallback.html) which uses a plain javascript to invoke the queries and displays the transcribed SwaggerSocket messages.

