echo-nodejs
==================
A node.js client that can call SwaggerSocket's Echo Sample service.
In order to run this sample, you need SwaggerSocket's Echo Sample or one of the other SwaggerSocket echo samples.

SwaggerSocket Echo Sample [download sample](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.wordnik%22%20AND%20a%3A%22swaggersocket-sample-echo%22) | [README](https://github.com/swagger-api/swagger-socket/blob/master/samples/swaggersocket-echo/README.txt)

For more details, refer to [https://github.com/swagger-api/swagger-socket](https://github.com/swagger-api/swagger-socket)

### Prerequisite
Go to the Download page mentioned above and download distribution.zip
Unzip this file into some directory.

For example, when using version 2.0.1, you execute the following commands at the Console

```bash
unzip swaggersocket-sample-echo-2.0.1-distribution.zip -d swaggersocket-sample-echo
cd swaggersocket-sample-echo
chmod a+x bin/nettosphere.sh
bin/nettosphere.sh
```


### Install

To install this client, you go to this sample directory and execute the following command at anothe Console

```bash
cd echo-nodejs
npm install
```

### Run

```bash
node echo-client.js
```

This will start an interactive shell shown below, where you can type in a message to be echoed at the message prompt.

```code
Using the target URL: http://localhost:8080/
Connecting ...
----------------------------
STATUS: OK
SwaggerSocket connected
----------------------------
message: Hello
Sending a request using uuid 1429267164385
> Response for Request: 1429267164385 is 'Hello'
message: 
```


If you are using an echo service running at another URL, you can set it as the command argument.
For example, if you are using osgi_cxf_echo_service, you can start this client with

```bash
node echo-client.js http://localhost:8181/cxf/RestContext/swaggersocket_echo/
```


