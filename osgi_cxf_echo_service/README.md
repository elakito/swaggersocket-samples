Apache CXF JAXRS SwaggerSocket echo service on Karaf
=================================================

This demo illustrates how to develop a simple CXF jaxrs based
service that can be invoked using SwaggerSocket. SwaggerSocket is
a protocol that allows any existing REST services to be invoked
over WebSocket. See further information on SwaggerSocket at its
project page at
[https://github.com/swagger-api/swagger-socket](https://github.com/swagger-api/swagger-socket)

This demo uses CXF's integrated Atmosphere feature where you can attach
the SwaggerSocket protocol interceptor to the CXF bus in its blueprint configuration file [context.xml](src/main/resources/OSGI-INF/blueprint/context.xml). The echo service itself is from the echo service of the SwaggerSocket samples [https://github.com/swagger-api/swagger-socket/tree/master/samples/swaggersocket-cxf-echo](https://github.com/swagger-api/swagger-socket/tree/master/samples/swaggersocket-cxf-echo).


Building
--------
From the base directory of this sample, the pom.xml file
is used to build and run the standalone unit test.

```bash
  mvn clean install
```

Running the demo in OSGi
------------------------
After building the sample, install the bundle to your karaf
container. The SwaggerSocket feature and the CXF websocket feature must be installed on your
karaf container.

If you do not have Karaf installed, download apache-karaf-3.0.3.tar.gz from one of the [mirror sites](http://www.apache.org/dyn/closer.cgi/karaf/3.0.3/apache-karaf-3.0.3.tar.gz) and unpack the archive.

```bash
$ wget -N http://ftp.halifax.rwth-aachen.de/apache/karaf/3.0.3/apache-karaf-3.0.3.tar.gz
$ tar -zxf apache-karaf-3.0.3.tar.gz
$ cd apache-karaf-3.0.3
```

#### Starting Karaf

```bash
$ bin/karaf

        __ __                  ____      
       / //_/____ __________ _/ __/      
      / ,<  / __ `/ ___/ __ `/ /_        
     / /| |/ /_/ / /  / /_/ / __/        
    /_/ |_|\__,_/_/   \__,_/_/         

  Apache Karaf (3.0.3)

Hit '<tab>' for a list of available commands
and '[cmd] --help' for help on a specific command.
Hit '<ctrl-d>' or type 'system:shutdown' or 'logout' to shutdown Karaf.

karaf@root()>
```
#### Note on using Karaf behind a firewall

If you do not have a direct access to the maven central repositories, you need to set Karaf's property
org.ops4j.pax.url.mvn.repositories to point to your local repository

To use your local repository http://nexus.mycorp.com:8081/nexus/content/groups/build.milestone, run the following commands.

```
config:edit org.ops4j.pax.url.mvn
config:property-set org.ops4j.pax.url.mvn.repositories http://nexus.mycorp.com:8081/nexus/content/groups/build.milestone
config:update
```

Alternatively, you can edit this property in file etc/org.ops4j.pax.url.mvn.cfg.

For further details, please refer to [Karaf User Guide](http://karaf.apache.org/manual/latest/users-guide/index.html).

#### Install SwaggerSocket feature

To install the SwaggerSocket feature, run the following karaf console commands.

```bash
feature:repo-add mvn:com.wordnik/swaggersocket-karaf-features/2.0.2/xml/features
feature:install swaggersocket-server
```

#### Install CXF WebSocket feature

To install the CXF websocket feature, run the following karaf console
commands. (Note we need CXF 3.0.5 or newer to get the integrated atmosphere support)

```bash
  feature:repo-add cxf 3.0.5
  feature:install cxf-jaxrs cxf-transports-websocket-server
```

#### Install this sample bundle

To install this sample bundle, run the karaf console command.

```bash
  install -s mvn:de.elakito.swaggersocket.samples/osgi-cxf-echo-service
```

Shown below is the output from running the above Karaf console commands.

```bash
karaf@root()> feature:repo-add mvn:com.wordnik/swaggersocket-karaf-features/2.0.2/xml/features
Adding feature url mvn:com.wordnik/swaggersocket-karaf-features/2.0.2/xml/features
karaf@root()> feature:install swaggersocket-server
karaf@root()> feature:repo-add cxf 3.0.5
Adding feature url mvn:org.apache.cxf.karaf/apache-cxf/3.0.5/xml/features
karaf@root()> feature:install cxf-jaxrs cxf-transports-websocket-server
Refreshing bundles org.ops4j.pax.web.pax-web-jetty (80), org.apache.geronimo.specs.geronimo-jaspic_1.0_spec (69), org.ops4j.pax.web.pax-web-runtime (79)
karaf@root()> install -s mvn:de.elakito.swaggersocket.samples/osgi-cxf-echo-service
Bundle ID: 120
karaf@root()> list
START LEVEL 100 , List Threshold: 50
 ID | State  | Lvl | Version        | Name                                                  
--------------------------------------------------------------------------------------------
 90 | Active |  80 | 2.2.7          | atmosphere-runtime                                    
 91 | Active |  80 | 2.0.2          | swaggersocket-protocol                                
 92 | Active |  80 | 2.0.2          | swaggersocket-server                                  
118 | Active |  80 | 2.2.6          | atmosphere-runtime                                    
120 | Active |  80 | 0.0.1.SNAPSHOT | de.elakito.swaggersocket.samples.osgi-cxf-echo-service
karaf@root()> 
```

You can verify the CXF bus and endpoint are correctly installed and started using cxf:list-busses and cxf:list-endpoints.

```bash
karaf@root()> cxf:list-busses
Name                                                                 State               
[de.elakito.swaggersocket.samples.osgi-cxf-echo-service-cxf35153316] [RUNNING           ]
karaf@root()> cxf:list-endpoints
Name                      State      Address                                                      BusID                                   
[SwaggerSocketResource  ] [Started ] [/RestContext/swaggersocket_echo                           ] [de.elakito.swaggersocket.samples.osgi-cxf-echo-service-cxf35153316]
karaf@root()> 
```

#### Testing this demo

Using Browser, open [http://localhost:8181/cxf](http://localhost:8181/cxf) to view the installed endpoint and open [http://localhost:8181/cxf/RestContext/swaggersocket_echo/](http://localhost:8181/cxf/RestContext/swaggersocket_echo/) to go to this sample's echo page.

Enter some text and publish it to the server. You can choose the operation implemented by the echo service.

To observe the detailed interaction, you can go to the [fallback page](http://localhost:8181/cxf/RestContext/swaggersocket_echo/fallback), which can display each SwaggerSocket messages that are sent and received.

You can also try the node.js client of sample [echo-nodjs](../echo-nodejs/README.md) with the target URL: http://localhost:8181/cxf/RestContext/swaggersocket_echo
