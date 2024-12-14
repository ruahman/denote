---
title:      "activemq-artemis"
date:       2024-12-11T14:34:12-04:00
tags:       ["activemq"]
identifier: "20241211T143412"
signature:  ""
---

Messaging systems allow you to loosely couple heterogeneous systems together

Unlike systems based on a Remote Procedure Call (RPC) pattern, messaging systems primarily use an asynchronous message passing pattern

this allows you to really take advantage of your hardware resources, 
minimizing the amount of threads blocking on IO operations, and to use your network bandwidth to its full capacity. 
With an RPC approach you have to wait for a response for each request you make

Messaging systems decouple the senders of messages from the consumers of messages. 
The senders and consumers of messages are completely independent and know nothing of each other. 

Messaging systems normally support two main styles of asynchronous messaging: 
*message queue messaging* (also known as *point-to-point* messaging) and *publish subscribe messaging*. 

# Point-to-Point
you send a message to a queue
then some time later the messaging system delivers the message to a consumer. 
The consumer then processes the message and when it is done, it acknowledges the message. 
Once the message is acknowledged it disappears from the queue and is not available to be delivered again

# Publish-Subscribe
g many senders can send messages to an entity on the server, often called a topic
There can be many subscriptions on a topic, a subscription is just another word for a consumer of a topic. 
Each subscription receives a copy of each message sent to the topic. This differs from the message queue pattern where each message is only consumed by a single consumer.
 
Apacde ActiveMQ Artemis clients,   interact with the Apache ActiveMQ Artemis broker. 

Apache ActiveMQ Artemis also provides different protocol implementations on the server so you can use respective clients for these protocols:
* AMQP
* MQTT
* STOMP
* OpenWire

The Apache ActiveMQ Artemis broker does not speak JMS and in fact does not know anything about JMS, it is a protocol agnostic messaging server designed to be used with multiple different protocols.


The normal stand-alone messaging broker configuration comprises a core messaging broker and a number of protocol managers that provide support for the various protocol mentioned earlier


# Supported Protocols

The broker has a pluggable protocol architecture. 
Protocol plugins come in the form of protocol modules. 
Each protocol module is included on the broker’s class path and loaded by the broker at boot time. 
The broker ships with 5 protocol modules out of the box.


## MQTT 

## STOMP
Stomp is a very simple text protocol for interoperating with messaging systems.
 theoretically any Stomp client can work with any messaging system that supports Stomp. 
Any client that supports the 1.0, 1.1, or 1.2 specification will be able to interact with Apache ActiveMQ Artemis.

## Configuring Acceptors
In order to make use of a particular protocol, a transport must be configured with the desired protocol enabled. 

The default configuration shipped with the ActiveMQ Artemis distribution comes with a number of acceptors already defined, one for each of the above protocols plus a generic acceptor that supports all protocols. 

 To enable protocols on a particular acceptor simply add the protocols url parameter to the acceptor url where the value is one or more protocols (separated by commas). If the protocols parameter is omitted from the url all protocols are enabled. 
 
MQTT
``` xml
<acceptors>
   <acceptor>tcp://localhost:1883?protocols=MQTT</acceptor>
</acceptors>
```

MQTT,AMQP
``` xml
<acceptors>
   <acceptor>tcp://localhost:5672?protocols=MQTT,AMQP</acceptor>
</acceptors>
```

all
``` xml
<acceptors>
   <acceptor>tcp://localhost:61616</acceptor>
</acceptors>
```

## AMQP
. By default there are acceptor elements configured to accept AMQP connections on ports 61616 and 5672.

## stomp
STOMP is a text-orientated wire protocol that allows STOMP clients to communicate with STOMP Brokers. Apache ActiveMQ Artemis supports STOMP 1.0, 1.1 and 1.2.

By default there are acceptor elements configured to accept STOMP connections on ports 61616 and 61613

# Docker
One of the simplest ways to get started with ActiveMQ Artemis is by using one of our Docker images.

 expose the main messaging port (i.e. 61616) and HTTP port (i.e. 8161)

`$ docker run --detach --name mycontainer -p 61616:61616 -p 8161:8161 --rm apache/activemq-artemis:latest-alpine`

Once the broker starts you can open the web management console at http://localhost:8161 and log in with the default username & password `artemis`

You can also use the `shell` command to interact with the running broker using the default username & password `artemis`

see logs
`docker logs -f mycontainer`

# environment variables

ARTEMIS_USER ::
	default is `artemis`

AREMIS_PASSWORD ::
    default is `artemis`
	
ANONYMOUS_LOGIN :
	allow anonymous logins

EXTRA_ARGS :
	additional argument setn to the artemis create command.
	defualt is  --http-host 0.0.0.0 --relax-jolokia 
	
	
these environment variable combine to run the followin command
`${ARTEMIS_HOME}/bin/artemis create --user ${ARTEMIS_USER} --password ${ARTEMIS_PASSWORD} --silent ${LOGIN_OPTION} ${EXTRA_ARGS}`

# volumn
The image will use the directory /var/lib/artemis-instance to hold the configuration and the data of the running broker

`docker run -it -p 61616:61616 -p 8161:8161 -v <broker folder on host>:/var/lib/artemis-instance apache/activemq-artemis:latest-alpine` 

# override files
You can use customized configuration for the ActiveMQ Artemis instance by replacing the files residing in the etc folder with the custom ones, e.g. broker.xml or artemis.profile. 

# using the server

This document will refer to the full path of the directory where the ActiveMQ distribution has been extracted to as ${ARTEMIS_HOME}.

file structure
``` shell
_ bin
|
|___ lib
|
|___ schema
|
|___ web
```

bin
binaries and scripts needed to run ActiveMQ Artemis.

lib
jars and libraries needed to run ActiveMQ Artemis

schema
XML Schemas used to validate ActiveMQ Artemis configuration files

web
The folder where the web context is loaded when the broker runs.

# Creating a Broker Instance 
A broker instance is the directory containing all the configuration and runtime data, such as logs and message journal, associated with a broker process. 

It is recommended that you do not create the instance directory under ${ARTEMIS_HOME}. This separation is encouraged so that you can more easily upgrade when the next version of ActiveMQ Artemis is released.

Before the broker is used, a broker instance must be created.

``` shell
$ cd /var/lib
$ ${ARTEMIS_HOME}/bin/artemis create mybroker
```

A broker instance directory will contain the following sub directories:

bin
holds execution scripts associated with this instance.

data
holds the data files used for storing persistent messages

etc
hold the instance configuration files

lib
holds any custom runtime Java dependencies like transformers, plugins, interceptors, etc.

log
holds rotating log files

tmp
holds temporary files that are safe to delete between broker runs

`./artemis create /usr/server` 

# starting and stopping a broker instance

run broker instance
`/var/lib/mybroker/bin/artemis run` 

stop broker instance
`/var/lib/mybroker/bin/artemis stop`

By default the etc/bootstrap.xml configuration is used. The configuration can be changed e.g. by running ./artemis run -- xml:path/to/bootstrap.xml or another config of your choosing.

Environment variables are used to provide ease of changing ports, hosts and data directories used and can be found in etc/artemis.profile on linux and etc\artemis.profile.cmd on Windows.

# configuration files 

These are the files you’re likely to find in the etc directory of a default broker instance

artemis.profile
system properties and JVM arguments (e.g. Xmx, Xms, etc.)

artemis-users.properties
user/password for the default

bootstrap.xml
embedded web server, security, location of broker.xml

# command line 

ActiveMQ Artemis has a Command Line Interface (CLI) that can used to manage a few aspects of the broker like instance creation, basic user management, queue & address management, etc.

There are two ways the CLI can be used:

Traditional CLI commands, e.g.: ./artemis [COMMAND] [PARAMETERS]

A custom shell that is accesssed using the ./artemis or ./artemis shell commands.

`Apache ActiveMQ Artemis > connect --user=myUser --password=myPass --url tcp://localhost:61616`

`$ ./artemis shell --user <username> --password <password> --url tcp://<hostname>:<port>`
