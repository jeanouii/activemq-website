---
layout: default_md
title: Version 5 Run Broker 
title-class: page-title-classic
type: classic
---

[Using ActiveMQ Classic 5](using-activemq-classic-5) > [Version 5 Run Broker](version-5-run-broker)


Running an ActiveMQ Classic Broker
==========================

Note if you want to use an **embedded broker** then see [How do I embed a Broker inside a Connection](how-do-i-embed-a-broker-inside-a-connection)

The [binary distribution](download) of ActiveMQ Classic comes with a script called 'activemq' which allows you to run a broker.  
For details regarding the activemq init script file review  [Unix Shell Script](unix-shell-script)  and  [ActiveMQ Classic Command Line Tools Reference](activemq-classic-command-line-tools-reference)

Typing the following will run an ActiveMQ Classic Broker using the out of the box configuration in the foreground
```
bin/activemq console
```
You can then use a [Broker Configuration URI](broker-configuration-uri) to specify how to start and configure your broker using a single URI. For example
```
bin/activemq console broker:(tcp://localhost:61616,network:static:tcp://remotehost:61616)?persistent=false&useJmx=true
```
Or you can a [Broker XBean URI](broker-xbean-uri) to customize the Message Broker using the [Xml Configuration](xml-configuration) to suit your needs. You can run a broker with a specific XML configuration as
```
bin/activemq console xbean:foo.xml
```
Or you can use a [Broker Properties URI](broker-properties-uri) to customize the Message Broker using a properties file; which avoids the dependency on Spring, xbean-spring and XML.
```
bin/activemq console properties:foo.properties
```

### Monitoring the broker

You can monitor ActiveMQ Classic using the [Web Console](web-console) by pointing your browser at

[http://localhost:8161/admin](http://localhost:8161/admin)

From ActiveMQ Classic 5.8 onwards the web apps is secured out of the box.  

The default username and password is admin/admin. You can configure this in the conf/jetty-real.properties file.

Or you can use the [JMX](jmx) support to view the running state of ActiveMQ Classic.

For more information see the file `docs/WebConsole-README.txt` in the distribution.

### Running the broker inside a Servlet Engine

See the source code (or WAR) of the [Web Console](web-console) for an example of how to run the broker inside a web application using Spring.

### Running the broker inside your J2EE Application Server

Whether its Apache Geronmio, JBoss, WebLogic or some other J2EE container you should be able to just reconfigure and then deploy the activemq-*.rar which is included in the binary distribution as a deployment unit in your app server. By default the rar is not configured to start an embedded broker. But by setting the brokerXmlConfig on the resource adapter configuration, the resource adapter will start an embedded broker.

For more details see [J2EE](j2ee)

### Running the broker from the source code

From the latest [checkout](source) of the code you can run a broker using the [ActiveMQ Classic Performance Plugin](activemq-classic-performance-module-users-manual)

### Running the broker from maven

You can download and install the ActiveMQ Classic Startup Maven Plugin via the following command if you are in a directory with a pom.xml. More detailed usage [here](maven2-activemq-broker-plugin)
```
mvn org.apache.activemq.tooling:maven-activemq-plugin:5.0-SNAPSHOT:run    
```
You can also include it the pom and run it using:
```
mvn activemq:run          
```
Handling JMS brokers going down
-------------------------------

A common requirement is that if the JMS broker goes down you want to automatically detect the failure and try to reconnect under the covers so that your application does not have to worry about reconnection.

There is detailed documentation on this in [Configuring Version 5 Transports](configuring-version-5-transports); briefly...

Just change your connection URI i to
```
failover:tcp://host:port
```
And the JMS client will auto-reconnect to the broker if it is shutdown and restarted later on.

