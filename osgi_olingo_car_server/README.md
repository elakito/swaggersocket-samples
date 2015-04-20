Apache Olingo OData SwaggerSocket enabled Cars service (OSGi)
=================================================

This demo is an OSGi version of the [SwaggetrSocked enabled Olingo Cars demo](https://github.com/elakito/swaggersocket-samples/blob/master/olingo_car_server/)

SwaggerSocket is
a protocol that allows any existing REST services to be invoked
over WebSocket. See further information on SwaggerSocket at its
project page
[https://github.com/swagger-api/swagger-socket](https://github.com/swagger-api/swagger-socket)

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

#### Install Olingo libs and other depdenent libs

For now, we install the individual bundles one by one. We can define feature olingo-server to install all the bundles at once in the future.

First, to install the depending bundles of olingo, run the following karaf console commands.

```bash
feature:install war
bundle:install -s mvn:commons-codec/commons-codec/1.9
bundle:install -s mvn:org.apache.commons/commons-lang3/3.3.2
bundle:install -s mvn:org.codehaus.woodstox/stax2-api/3.1.4
bundle:install -s mvn:com.fasterxml/aalto-xml/0.9.8
bundle:install -s 'wrap:mvn:org.antlr/antlr4-runtime/4.1/$Bundle-SymbolicName=antlr4-runtime&Bundle-Version=4.1&Export-Package=org.antlr.v4.runtime*'
bundle:install -s mvn:com.fasterxml.jackson.core/jackson-core/2.4.1
bundle:install -s mvn:com.fasterxml.jackson.core/jackson-annotations/2.4.1
bundle:install -s mvn:com.fasterxml.jackson.core/jackson-databind/2.4.1
```

Now, install the olingo bundles by running the following commands. Note that
we assume we are using the patched version regarding OLINGO-632.

```bash
bundle:install -s mvn:org.apache.olingo/odata-commons-api/4.0.0-beta-03-SNAPSHOT
bundle:install -s mvn:org.apache.olingo/odata-commons-core/4.0.0-beta-03-SNAPSHOT
bundle:install -s mvn:org.apache.olingo/odata-server-api/4.0.0-beta-03-SNAPSHOT
bundle:install -s mvn:org.apache.olingo/odata-server-core/4.0.0-beta-03-SNAPSHOT
```

#### Install SwaggerSocket feature

To install the SwaggerSocket feature, run the following karaf console commands.

```bash
feature:repo-add mvn:com.wordnik/swaggersocket-karaf-features/2.0.1/xml/features
feature:install swaggersocket-server
```

#### Install this sample bundle

To install this sample bundle, run the karaf console command.

```bash
bundle:install -s mvn:de.elakito.swaggersocket.samples/osgi-olingo-car-server
```

Shown below is the output from running the above Karaf console commands.

```bash
karaf@root()> feature:install war
karaf@root()> bundle:install -s mvn:commons-codec/commons-codec/1.9
Bundle ID: 97
karaf@root()> bundle:install -s mvn:org.apache.commons/commons-lang3/3.3.2
Bundle ID: 98
karaf@root()> bundle:install -s mvn:org.codehaus.woodstox/stax2-api/3.1.4
Bundle ID: 99
karaf@root()> bundle:install -s mvn:com.fasterxml/aalto-xml/0.9.8
Bundle ID: 100
karaf@root()> bundle:install -s 'wrap:mvn:org.antlr/antlr4-runtime/4.1/$Bundle-SymbolicName=antlr4-runtime&Bundle-Version=4.1&Export-Package=org.antlr.v4.runtime*'
Bundle ID: 101
karaf@root()> 
karaf@root()> bundle:install -s mvn:com.fasterxml.jackson.core/jackson-core/2.4.1
Bundle ID: 102
karaf@root()> bundle:install -s mvn:com.fasterxml.jackson.core/jackson-annotations/2.4.1
Bundle ID: 103
karaf@root()> bundle:install -s mvn:com.fasterxml.jackson.core/jackson-databind/2.4.1
Bundle ID: 104
karaf@root()> 
karaf@root()> bundle:install -s mvn:org.apache.olingo/odata-commons-api/4.0.0-beta-03-SNAPSHOT
Bundle ID: 105
karaf@root()> bundle:install -s mvn:org.apache.olingo/odata-commons-core/4.0.0-beta-03-SNAPSHOT
Bundle ID: 106
karaf@root()> bundle:install -s mvn:org.apache.olingo/odata-server-api/4.0.0-beta-03-SNAPSHOT
Bundle ID: 107
karaf@root()> bundle:install -s mvn:org.apache.olingo/odata-server-core/4.0.0-beta-03-SNAPSHOT
Bundle ID: 108
karaf@root()> feature:repo-add mvn:com.wordnik/swaggersocket-karaf-features/2.0.1/xml/features
Adding feature url mvn:com.wordnik/swaggersocket-karaf-features/2.0.1/xml/features
karaf@root()> feature:install swaggersocket-server
Refreshing bundles org.eclipse.jetty.aggregate.jetty-all-server (71)
karaf@root()> bundle:install -s mvn:de.elakito.swaggersocket.samples/osgi-olingo-car-server
Bundle ID: 116
karaf@root()>
```

To verify if the sample is correctly installed and running, use list and web:list to see its bundle status and its web context is registered.

```bash
karaf@root()> list
START LEVEL 100 , List Threshold: 50
 ID | State  | Lvl | Version                | Name                                                   
-----------------------------------------------------------------------------------------------------
 97 | Active |  80 | 1.9.0                  | Apache Commons Codec                                   
 98 | Active |  80 | 3.3.2                  | Apache Commons Lang                                    
 99 | Active |  80 | 3.1.4                  | Stax2 API                                              
100 | Active |  80 | 0.9.8                  | aalto-xml                                              
101 | Active |  80 | 4.1                    | antlr4-runtime                                         
102 | Active |  80 | 2.4.1                  | Jackson-core                                           
103 | Active |  80 | 2.4.1                  | Jackson-annotations                                    
104 | Active |  80 | 2.4.1                  | jackson-databind                                       
105 | Active |  80 | 4.0.0.beta-03-SNAPSHOT | odata-commons-api                                      
106 | Active |  80 | 4.0.0.beta-03-SNAPSHOT | odata-commons-core                                     
107 | Active |  80 | 4.0.0.beta-03-SNAPSHOT | odata-server-api                                       
108 | Active |  80 | 4.0.0.beta-03-SNAPSHOT | odata-server-core                                      
113 | Active |  80 | 2.2.6                  | atmosphere-runtime                                     
114 | Active |  80 | 2.0.1                  | swaggersocket-protocol                                 
115 | Active |  80 | 2.0.1                  | swaggersocket-server                                   
116 | Active |  80 | 0.0.1.SNAPSHOT         | de.elakito.swaggersocket.samples.osgi-olingo-car-server
karaf@root()> web:list
ID  | State       | Web-State   | Level | Web-ContextPath            | Name                                                                    
-----------------------------------------------------------------------------------------------------------------------------------------------
116 | Active      | Deployed    | 80    | /swaggersocket-olingo-cars | de.elakito.swaggersocket.samples.osgi-olingo-car-server (0.0.1.SNAPSHOT)
karaf@root()> 
```

#### Test this sample

Using Browser, open [http://localhost:8181/swaggersocket-olingo-cars](http://localhost:8181/swaggersocket-olingo-cars) to access the default page and see some available
queries. You can invoke these queries over HTTP by clicking on those links or directly typing
in the queries in Browser's URL field.

To use SwaggerSocket to invoke these queries, open [http://localhost:8181/swaggersocket-olingo-cars/index.html](http://localhost:8181/swaggersocket-olingo-cars/index.html).

The page uses swaggersocket.js to connect to the server and invoke
the queries asynchronously over a single WebSocket. To view which SwaggerSocket messages are sent and received, you can go to
the fallback page at [http://localhost:8181/swaggersocket-olingo-cars/index_fallback.html](http://localhost:8181/swaggersocket-olingo-cars/index_fallback.html) which uses a plain javascript to invoke the queries and displays the transcribed SwaggerSocket messages.

