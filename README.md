# Eclipse Paho JavaScript client

[![Build Status](https://travis-ci.org/eclipse/paho.mqtt.javascript.svg?branch=develop)](https://travis-ci.org/eclipse/paho.mqtt.javascript)

The Paho JavaScript Client is an MQTT browser-based client library written in Javascript that uses WebSockets to connect to an MQTT Broker.

## Project description:

The Paho project has been created to provide reliable open-source implementations of open and standard messaging protocols aimed at new, existing, and emerging applications for Machine-to-Machine (M2M) and Internet of Things (IoT).
Paho reflects the inherent physical and cost constraints of device connectivity. Its objectives include effective levels of decoupling between devices and applications, designed to keep markets open and encourage the rapid growth of scalable Web and Enterprise middleware and applications.

## Links

- Project Website: [https://www.eclipse.org/paho](https://www.eclipse.org/paho)
- Eclipse Project Information: [https://projects.eclipse.org/projects/iot.paho](https://projects.eclipse.org/projects/iot.paho)
- Paho Javascript Client Page: [https://eclipse.org/paho/clients/js/](https://eclipse.org/paho/clients/js)
- GitHub: [https://github.com/eclipse/paho.mqtt.javascript](https://github.com/eclipse/paho.mqtt.javascript)
- Twitter: [@eclipsepaho](https://twitter.com/eclipsepaho)
- Issues: [github.com/eclipse/paho.mqtt.javascript/issues](https://github.com/eclipse/paho.mqtt.javascript/issues)
- Mailing-list: [https://dev.eclipse.org/mailman/listinfo/paho-dev](https://dev.eclipse.org/mailman/listinfo/paho-dev)


## Using the Paho Javascript Client


### Downloading

A zip file containing the full and a minified version the Javascript client can be downloaded from the [Paho downloads page](https://projects.eclipse.org/projects/iot.paho/downloads)

Alternatively the Javascript client can be downloaded directly from the projects git repository: [https://raw.githubusercontent.com/eclipse/paho.mqtt.javascript/master/src/paho-mqtt.js](https://raw.githubusercontent.com/eclipse/paho.mqtt.javascript/master/src/paho-mqtt.js).

Please **do not** link directly to this url from your application.

### Building from source

There are two active branches on the Paho Java git repository, ```master``` which is used to produce stable releases, and ```develop``` where active development is carried out. By default cloning the git repository will download the ```master``` branch, to build from develop make sure you switch to the remote branch: ```git checkout -b develop remotes/origin/develop```

The project contains a maven based build that produces a minified version of the client, runs unit tests and generates it's documentation.

To run the build:

```
$ mvn
```

The output of the build is copied to the ```target``` directory.

### Tests

The client uses the [Jasmine](http://jasmine.github.io/) test framework. The tests for the client are in:

```
src/tests
```

To run the tests with maven, use the following command:
```
$ mvn test
```
The parameters passed in should be modified to match the broker instance being tested against.

### Documentation

Reference documentation is online at: [http://www.eclipse.org/paho/files/jsdoc/index.html](http://www.eclipse.org/paho/files/jsdoc/index.html)

### Compatibility

The client should work in any browser fully supporting WebSockets, [http://caniuse.com/websockets](http://caniuse.com/websockets) lists browser compatibility.

## Getting Started

The included code below is a very basic sample that connects to a server using WebSockets and subscribes to the topic ```World```, once subscribed, it then publishes the message ```Hello``` to that topic. Any messages that come into the subscribed topic will be printed to the Javascript console.

This requires the use of a broker that supports WebSockets natively, or the use of a gateway that can forward between WebSockets and TCP.
### web端示例
超强健壮。断网、宕机自动重连。
```JS
    var options = {
      timeout: 3,
      userName: "name",
      password: "pwd",
      mqttVersion: 4,
      keepAliveInterval: 5,
      onSuccess: onConnect,
      onFailure: onFailure
    };
     options.useSSL = true;
     var wsport = 3301;
     
     //如果是小程序， -----------------------------    ------       web 可以改成wxMini
     var client = new Paho.Client("192.168.1.135", wsport, "/ws", "web_" + options.userName + "-" + parseInt(Math.random() * 100, 10));
    client.onConnectionLost = onConnectionLost;
    client.onMessageArrived = onMessageArrived;
    
    //打印日志，正式环境去掉
     client.trace=function(obj){
      console.log(obj.severity + "  " + obj.message);
    }

    function connToMqtt() {
      client.connect(options);
    }
    connToMqtt();
     function onConnect() {
      // Once a connection has been made, make a subscription and send a message.
      console.log("onConnect");
      client.subscribe("hole",{
        onSuccess:function(){
          client.publish("hole","*********通则不痛，痛则不通************");
        }
      });
    }
    // called when the client loses its connection
    function onConnectionLost(responseObject) {
      if (responseObject.errorCode !== 0) {
        console.log("onConnectionLost:" + responseObject.errorMessage);
      }
      connToMqtt();
    }

    // called when a message arrives
    function onMessageArrived(message) {
      console.log(message.payloadString);
      
    }
    function onFailure(responseObject) {
      console.log(responseObject);
    }
```
### 小程端示例
```
//将paho.mqtt-WxMini.js拷贝到小程序utils文件夹下，声明
var Paho = require('../../utils/mqttjs')

//接下来的代码和上面web端的一模一样了
```
## Breaking Changes

Previously the Client's Namepsace was `Paho.MQTT`, as of version 1.1.0 (develop branch) this has now been simplified to `Paho`.
You should be able to simply do a find and replace in your code to resolve this, for example all instances of `Paho.MQTT.Client` will now be `Paho.Client` and `Paho.MQTT.Message` will be `Paho.Message`.
