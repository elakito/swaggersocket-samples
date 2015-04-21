Apache CXF JAXRS SwaggerSocket echo service using Spring
=================================================

This demo illustrates how to develop a simple CXF jaxrs based
service that can be invoked using SwaggerSocket. SwaggerSocket is
a protocol that allows any existing REST services to be invoked
over WebSocket. See further information on SwaggerSocket at its
project page at
[https://github.com/swagger-api/swagger-socket](https://github.com/swagger-api/swagger-socket)

This demo uses CXF's integrated Atmosphere feature where you can attach
the SwaggerSocket protocol interceptor to the CXF bus in its configuration file [context.xml](osgi_cxf_echo_service/src/main/resources/OSGI-INF/blueprint/context.xml). The echo service itself is from the echo service of the SwaggerSocket samples [https://github.com/swagger-api/swagger-socket/tree/master/samples/swaggersocket-cxf-echo](https://github.com/swagger-api/swagger-socket/tree/master/samples/swaggersocket-cxf-echo).


Building
--------
From the base directory of this sample, the pom.xml file
is used to build and run the standalone unit test.

```bash
  mvn clean install
```

Running the demo
------------------------
You can use the embedded jetty to run this demo by running the following command.

```bash
mvn jetty:run
```

#### Testing this demo

Using Browser, open [http://localhost:8080/RestContext/swaggersocket_echo/](http://localhost:8080/RestContext/swaggersocket_echo/) to go to this sample's echo page.

Enter some text and publish it to the server. You can choose the operation implemented by the echo service.

To observe the detailed interaction, you can go to the [fallback page](http://localhost:8080/RestContext/swaggersocket_echo/fallback), which can display each SwaggerSocket messages that are sent and received.

