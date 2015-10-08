JavaScript JMS Cookbook for KAAZING Gateway  ![This feature is available in KAAZING Gateway - Enterprise Edition](images/enterprise-feature.png)
=================================================

This document contains the following sections:

-   [Introduction](#introduction)
    -   [Who Should Read This Document?](#who-should-read-this-document)
    -   [How To Use This Cookbook](#how-to-use-this-cookbook)
    -   [Components Used In This Cookbook](#components-used-in-this-cookbook)
-   [Beginning a New Client Application](#beginning-a-new-client-application)
-   [Establishing Connections to the Server](#establishing-connections-to-the-server)
-   [Receiving Messages from a Topic or Queue](#receiving-messages-from-a-topic-or-queue)
-   [Sending Messages to JMS Topics and Queues](#sending-messages-to-jms-topics-and-queues)
-   [Implementing Two-Way Communication](#implementing-two-way-communication)
-   [Reference](#reference)
-   [Summary](#summary)

Introduction
------------

KAAZING Gateway contains JavaScript JMS client libraries to communicate with any JMS-compliant message broker, such as Apache ActiveMQ. The libraries allow you to integrate web-based applications with standard, message-driven enterprise applications. This document provides examples of the typical steps in creating the web-based application.

**Note:** See the [JavaScript JMS Client API](http://developer.kaazing.com/documentation/jms/4.0/apidoc/client/javascript/jms/index.md) for the client libraries needed for developing web-based applications.

### Who Should Read This Document?

This document is written for:

-   System architects interested in understanding how easily KAAZING Gateway applications can be integrated into their existing messaging systems.
-   Javascript developers assigned the task of writing the client-side, web-based JMS applications.

JavaScript developers should be familiar with the following Javascript techniques in order to understand the development best practices demonstrated in this topic:

-   Instantiating classes using the [`new` operator](https://developer.mozilla.org/en/JavaScript/Reference/Operators/new). When using the JavaScript JMS Client API, the `new` keyword is used to create an instance of one of the predefined constructors, such as [JmsConnectionFactory](http://developer.kaazing.com/documentation/jms/4.0/apidoc/client/javascript/jms/JmsConnectionFactory.md "JsDoc: JmsConnectionFactory").
-   Writing [anonymous functions](http://helephant.com/2008/08/23/javascript-anonymous-functions/) (unnamed functions that execute immediately upon definition) and using them as callbacks. A callback function is called when a condition is met, such as when an event occurs, as opposed to a function that is called immediately. For example, you might create a function that is called when the connection is ready to use and data is flowing.
-   Handling errors with [**try**…**catch**](https://developer.mozilla.org/en/JavaScript/Reference/Statements/try...catch) blocks.

**Notes:**
 
-   We also recommend the server-side [JMS Tutorial](http://docs.oracle.com/javaee/1.3/jms/tutorial/index.html) for understanding server-side JMS development.
-   See [About KAAZING Gateway - Enterprise Edition](../about/about.md) to learm more about KAAZING Gateway - Enterprise Edition.

### <a name="usage"></a>How To Use This Cookbook

1.  To create your new application, start with the first two sections, [Beginning a New Client Application](#beginning-a-new-client-application) and [Establishing Connections to the Server](#establishing-connections-to-the-server).
2.  Determine which topics and queues are used by your server-side application. You need to know the names of the topics and queues created by the JMS provider in order to create identities using the built-in methods of the JavaScript JMS Client API.
3.  Use the sections on receiving and sending messages to add the appropriate code to your application:
    -   To configure your application to **receive** messages, use the code in [Receiving Messages from a Topic or Queue](#receiving-messages-from-a-topic-or-queue).
    -   To configure your application to **send** messages, use the code in [Sending Messages to JMS Topics and Queues](#sending-messages-to-jms-topics-and-queues).
    -   To configure your application to **send commands** to the server and **receive individual responses**, use the code in [Implementing Two-Way Communication](#implementing-two-way-communication).

### Components Used In This Cookbook

This cookbook uses the following components:

-   This cookbook assumes that you have a JMS-compliant message broker that you can use to test the code you implement in your client application. The cookbook does not contain any JMS-compliant message broker configuration steps.
-   A text editor or IDE for implementing the JavaScript and HTML examples.
-   KAAZING Gateway. See [Setting Up KAAZING Gateway](https://github.com/kaazing/gateway/blob/develop/doc/about/setup-guide.md) for information about configuring the Gateway.

Beginning a New Client Application
----------------------------------

Every KAAZING Gateway installation has a `base` directory (for example, in Windows, `C:\GATEWAY_HOME\web\base`) where applications reside. Each application must have its own directory and copy of the JavaScript client libraries. This section walks you through creating a client application directory and populating it with the required libraries and starter code.

### Create a Client Application Directory

1.  Navigate to `GATEWAY_HOME/web/base` and create a new folder named after your application (for example, *myapp*).
2.  Copy the `GATEWAY_HOME/lib/client/javascript` folder into your new application folder and change its name from **javascript** to **lib** (for example, `GATEWAY_HOME/web/base/myapp/lib`).

### Create Your Starting Files

1.  Create a file named **index.md** in your application folder and paste the following template into it:

    ``` xml
    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8">
      <title>My application</title>

      <!-- Optional: Enable Internet Explorer 6/7 cross-origin messaging -->
      <meta name="kaazing:postMessageBridgeURL" content="PostMessageBridge.md">

      <!-- Reference the WebSocket and JavaScript JMS libraries in the lib folder -->
      <script src="lib/WebSocket.js"></script>
      <script src="lib/jms/JmsClient.js"></script>

      <!-- Reference the JavaScript file you will create -->
      <script src="app.js"></script>

      <!--
      Create a function to run when the page loads.
      This function will be expanded later.
      -->
      <script>
        /* TODO: Change URL as needed */
        var url = "ws://localhost:8000/jms";    
        window.onload = function() {
          beginConnection(url);
        }
      </script>
    </head>

    <body>
    <!-- TODO: Your page body goes here -->
    </body>
    </html>
    ```

2.  Customize the title and body of the page. See lines 2 and 30 in the above code example.
3.  Create a JavaScript file named **app.js** in the same directory as **index.md**.
4.  Follow the directions in [Establishing connections to the server](#establishing-connections-to-the-server) to prepare **app.js**.

Establishing Connections to the Server
--------------------------------------

This section shows you how to add standard boilerplate for creating a connection from your application to the Gateway and to your back-end JMS-compliant message broker. The code needed to create a JMS Connection via WebSocket from your application to the Gateway and the JMS-compliant message broker is standard, boilerplate code. Every application has a single `JmsConnectionFactory` and at least one `Connection` and `Session`. (The [Connection](http://developer.kaazing.com/documentation/jms/4.0/apidoc/client/javascript/jms/Connection.md "JsDoc: Connection") object is a client's active connection to its JMS provider. The [Session](http://developer.kaazing.com/documentation/jms/4.0/apidoc/client/javascript/jms/Session.md "JsDoc: Session") object is a single-threaded context for producing and consuming messages.) Once the connection is configured, you can modify the Connection and Session as needed.

**Note:** To help you understand the following example, review the following API components and the jms service: 
-   [jms](../admin-reference/r_conf_jms.md#jms)
-   [Session](http://developer.kaazing.com/documentation/jms/4.0/apidoc/client/javascript/jms/Session.md)
-   [Connection](http://developer.kaazing.com/documentation/jms/4.0/apidoc/client/javascript/jms/Connection.md)
-   [JmsConnectionFactory](http://developer.kaazing.com/documentation/jms/4.0/apidoc/client/javascript/jms/JmsConnectionFactory.md)
-   [ConnectionFuture](http://developer.kaazing.com/documentation/jms/4.0/apidoc/client/javascript/jms/ConnectionFuture.md)

1.  Add this Javascript code to your **app.js** file:

    ``` js
    // Create a variable to store the connection            
    var connection = null;

    /*
    Create a function for creating a session and setting up 
    Topics, Queues, Consumers, Providers, and Listeners.
    The following function is called when the connection has been created
    but before starting the flow of data. 
    */
    function setUp() {
        // Typical session options shown.
        var session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        // Add your code here to set up Topics, Queues, Consumers, Providers, and Listeners.
    }

    // The following function is called when the connection is ready to use and data is flowing.
    function connectionStarted() {
        // Add code here when you need to do something after the connection starts.
    }

    // This function is called from the index.md page when the page loads
    function beginConnection(url) {
        // If the connection already exists, call the function for handling running connections.
        if (connection) {
            connectionStarted();
        } else {
            /* 
            Create a new object for the JMS connection to a JMS provider via WebSocket
            using the built-in constructor JmsConnectionFactory.
            */
            var factory = new JmsConnectionFactory(url);
            /*
            Create the actual JMS Connection via WebSocket using the createConnection() method.
            When the connection is created, the callback function with the try...catch is invoked.
            */
            var future = factory.createConnection(function() {
                try {
                    /*
                    Once the callback is invoked, client applications should use
                    ConnectionFuture.getValue() to fetch the actual connection.
                    getValue() returns the value from an asynchronous operation.
                    */
                    connection = future.getValue();
                    /* 
                    With the connection established, but before starting the flow of data,
                    create the session.
                    */
                    setUp();
                    /*
                    Call the function to use when the connection
                    is ready to use and data is flowing.
                    */
                    connection.start(connectionStarted);
                } catch(e) {
                    alert(e.message);
                }
            });
        }
    }
    }
    ```

    The call to `factory.createConnection` deserves some explanation. Creating a connection takes time, and you do not want to block the browser while this is happening. The `factory.createConnection` method solves the problem in three steps:

    1.  `createConnection` returns a `ConnectionFuture` object immediately. This is a placeholder object that will be filled in when the connection is ready.
    2.  `createConnection` also takes a callback function that will be called when the connection is ready.
    3.  The callback should call `getValue()` on the `ConnectionFuture` to retrieve the actual Connection object.

2.  Customize the `setUp()` function to create topics, queues, listeners, and producers.
3.  Customize the `connectionStarted()` function if you need to do something right after the connection starts such as [Sending Messages to JMS Topics and Queues](#sending-messages-to-jms-topics-and-queues).
4.  Call `beginConnection()` with a WebSocket URL (for example, `ws://example.com:8000/jms`) after the page has loaded.

**Notes:**
 
-   The `connection` variable is defined as a global. If you want to avoid using globals, package this code into a Javascript class or use the [module pattern](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#modulepatternjavascript).
-   The error handling (`catch(e)`) is in the code as a placeholder. You will want to improve this error handling to suit your needs.
-   You might need to configure the Gateway to accept incoming connections on your selected port (such as port 8000):

1.  Open the file `GATEWAY_HOME/conf/gateway-config.xml`.
2.  Under the `jms` service entry, add a second `<accept>` statement:

    `<accept>ws://${gateway.hostname}:8000/jms/</accept>`

    The section should now look like the following:

    ``` xml
            <service>
                <accept>ws://${gateway.hostname}:${gateway.extras.port}/jms</accept>
                <accept>ws://${gateway.hostname}:8000/jms</accept>
                <type>jms</type>
            .
            .
            .
            </service>
    ```

3.  According to the JMS API, the `connection.start()` method is called when the application is ready to receive messages. It does not have any effect on sending the messages. You can create a producer from the session and send message without starting the connection. For more information, see [Connection](http://docs.oracle.com/javaee/1.4/api/javax/jms/Connection.html) in the JMS API.

Receiving Messages from a Topic or Queue
----------------------------------------

Typically, an application listens for JMS Topics and Queues created by the server and might create *temporary* topics and queues client-side. A client-side **Listener** passes the incoming messages to a callback function.

**Note:** To help you understand the following example, review the following API components: 
-   [Session](http://developer.kaazing.com/documentation/jms/4.0/apidoc/client/javascript/jms/Session.md)
-   [MessageConsumer](http://developer.kaazing.com/documentation/jms/4.0/apidoc/client/javascript/jms/MessageConsumer.md)
-   [Message](http://developer.kaazing.com/documentation/jms/4.0/apidoc/client/javascript/jms/Message.md)

``` js
// Creates a topic identity given a Topic name.
var topic = session.createTopic("topic.stock");
/*
Creates a MessageConsumer object for the specified destination.
A client uses a MessageConsumer object to receive messages 
that have been published to a destination.
*/
var consumer = session.createConsumer(topic);
// Sets the message consumer's MessageListener.
consumer.setMessageListener(onMessage);
```

The message listener takes a JMS message as a parameter and can use the JMS API calls to read values:

``` js
/*
The Message interface is the root interface of all JMS messages.
It defines the message header and the acknowledge method used for all messages.
*/
function onMessage(message) {
  // message.getStringProperty('status');
}
```

### Integrating Topics and Queues With Your Application

1.  Review the [Session](http://developer.kaazing.com/documentation/jms/4.0/apidoc/client/javascript/jms/Session.md "JsDoc: Session") documentation for createTopic, createQueue, createTemporaryTopic, createTemporaryQueue, and createConsumer.
2.  Using the [connection boilerplate code](#establishing-connections-to-the-server), create the topics, queues, listeners, etc. inside the `setUp()` function:

    ``` js
    function setUp() {
      // Typical session options shown.
      var session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
      // Add your code here to set up Topics, Queues, Consumers, Providers, and Listeners
      var topic = session.createTopic("topic.stock");
      var consumer = session.createConsumer(topic);
      consumer.setMessageListener(onMessage);
    }
    ```

3.  Add the message listeners at the same level as the `setUp()` function in the [connection boilerplate code](#establishing-connections-to-the-server):

    ``` js
      consumer.setMessageListener(onMessage);
    }

    function onMessage(message) {
    }
    ```

Sending Messages to JMS Topics and Queues
-----------------------------------------

Clients send messages to JMS Topics and Queues established by the server.

**Note:** To help you understand the following example, review the following API components: 
-   [Session](http://developer.kaazing.com/documentation/jms/4.0/apidoc/client/javascript/jms/Session.md)
-   [Message](http://developer.kaazing.com/documentation/jms/4.0/apidoc/client/javascript/jms/Message.md)

``` js
var commandQueue = session.createQueue("/queue.command");
var commandProducer = session.createProducer(commandQueue);
var message = session.createTextMessage('do something');
commandProducer.send(message);
```

The `send()` method accepts an optional function to call after sending the message:

``` js
commandProducer.send(message, function() {
  // e.g. send another message. You still have access to the session here.
});
```

Note that `createTextMessage` sets the message payload. You can also set individual properties on the message:

``` js
message.setStringProperty('command', 'login');
```

### Integrating Messaging Sending With Your Application

1.  Review the [Session](http://developer.kaazing.com/documentation/jms/4.0/apidoc/client/javascript/jms/Session.md "JsDoc: Session") documentation for createTopic, createQueue, and createProducer.
2.  Using the [connection boilerplate code](#establishing-connections-to-the-server), create the topics, queues, producers, etc. inside the `setUp()` function:

    ``` js
    function setUp () {
      // Typical session options shown.
      var session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
      // Add your code here to set up Topics, Queues, Consumers, Providers, and Listeners
      var commandQueue = session.createQueue("/queue.command");
      var commandProducer = session.createProducer(commandQueue);
    }
    ```

3.  Create and produce your messages as needed. Note: You will need the `session` value.

Implementing Two-Way Communication
----------------------------------

This section provides examples of how to add two-way communication between your client and your JMS-compliant message broker. Many clients will need to send requests to the broker and receive individual responses. The easiest way to do this is with a public command queue on the broker and a temporary queue on the client (for responses). In cases where JMS messages have a
`reply-to` field, the client sets the reply address back to its temporary queue.

**Note:** To help you understand the following example, review the following API components: 
-   [Session](http://developer.kaazing.com/documentation/jms/4.0/apidoc/client/javascript/jms/Session.md)
-   [Message](http://developer.kaazing.com/documentation/jms/4.0/apidoc/client/javascript/jms/Message.md)

``` js
var sendCount = 0;

// Configure your application to receive responses
var responseQueue = session.createTemporaryQueue();
var responseConsumer = session.createConsumer(responseQueue);
responseConsumer.setMessageListener(onCommandResponse);

// Construct the message to send to the server
var commandMessage = session.createTextMessage('');
commandMessage.setStringProperty('command', command);

// Send the message to the server
commandMessage.setJMSReplyTo(responseQueue);
commandMessage.setJMSCorrelationID('cmd-' + sendCount++);
commandProducer.send(commandMessage); 

// Handle the response from the server when it arrives
function onCommandResponse(response) {
    try {
        // The following string properties must match 
        // the strings that the server-side code supplies
        var status = response.getStringProperty('status');
        if (status != 'ok')
            return;
        var result = response.getStringProperty('result');
        // Use the data for the purposes of your application
    } catch (exception) {
        window.alert(exception);
    }
}
```

### Integrating Two-Way Communication With Your Application

In this section, we'll create topics and queues to send to the JMS-compliant message broker, and then add a message listener for responses.

1.  Create a global counter for the number of sent messages. This step is optional, but it is useful for debugging. The temporary queue is added here to simplify sending messages later.

    ``` js
    var connection;
    var sendCount = 0;
    var responseQueue;
    ```

2.  Using the [connection boilerplate code](#establishing-connections-to-the-server), edit the `setUp()` function to create the required topics and queues:

    ``` js
    function setUp () {
      var session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
      var commandQueue = session.createQueue("/queue.command");
      var commandProducer = session.createProducer(commandQueue);

      responseQueue = session.createTemporaryQueue();
      var responseConsumer = session.createConsumer(responseQueue);
      responseConsumer.setMessageListener(onCommandResponse);
    }
    ```

3.  Add the message listener for the command responses as a peer of `setUp()`:

    ``` js
      responseConsumer.setMessageListener(onCommandResponse);
    }

    function onCommandResponse(response) {
    }
    ```

### Typical Server-Side Code

The server should extract the JmsReplyTo value ([getJMSReplyTo](http://developer.kaazing.com/documentation/jms/4.0/apidoc/client/javascript/jms/Message.md#getJMSReplyTo)) and use it as the address for the return message (lines 2, 4, and 10.) Also copy the correlation ID ([getJMSCorrelationID](http://developer.kaazing.com/documentation/jms/4.0/apidoc/client/javascript/jms/Message.md#getJMSCorrelationID "JsDoc: Message")) from the original message to the response when available (line 7).

**Note:** To help you understand the following example, review the following JMS client and server-side components:
 
-   [Message](http://developer.kaazing.com/documentation/jms/4.0/apidoc/client/javascript/jms/Message.md)
-   *[JMS Tutorial](http://docs.oracle.com/javaee/1.3/jms/tutorial/index.html)*

``` js
void sendResponse(Message commandMessage, String command, String status) throws JMSException {
    Destination responseQueue = commandMessage.getJMSReplyTo();
    if ( responseQueue != null ) {
        MessageProducer producer = _commandSession.createProducer(responseQueue);
        TextMessage response = _commandSession.createTextMessage();
        response.setText("-");
        response.setJMSCorrelationID(commandMessage.getJMSCorrelationID());
        response.setStringProperty("command", command);
        response.setStringProperty("status", status);
        producer.send(response, DeliveryMode.NON_PERSISTENT, Message.DEFAULT_PRIORITY, 0L);
    }
}
```

Reference
---------

### The JMS programming model

The JMS client library is modeled on the [Java Message Service](http://docs.oracle.com/javaee/1.3/jms/tutorial/index.html) APIs:

| Object             | Role                                         | Created by                                       |
|--------------------|----------------------------------------------|--------------------------------------------------|
| Connection Factory | Manages connections to the server            | Call `new JmsConnectionFactory          `        |
| Connection         | Handles multiple message streams             | Ask the factory: `factory.createConnection`      |
| Session            | Handles a group of topics and queues         | Ask the connection: `connection.createSession`   |
| Consumer           | Reads messages from a Topic or Queue         | Ask the session: `session.createConsumer(topic)` |
| Producer           | Sends messages to a Topic or Queue           | Ask the session: `session.createProducer(topic)` |
| Topic              | Holds a reference to a JMS topic (on server) | Ask the session: `session.createTopic`           |
| Temporary Topic    | Holds a reference to a JMS topic (on client) | Ask the session: `session.createTemporaryTopic`  |
| Queue              | Holds a reference to a JMS queue (on server) | Ask the session: `session.createQueue`           |
| Temporary Queue    | Holds a reference to a JMS queue (on client) | Ask the session: `session.createTemporaryQueue`  |

### Debugging

If you have trouble getting your code to work, try these debugging strategies:

1.  To check for potential Javascript language errors, open the browser’s console and check for error messages.
2.  To check for problems with JMS subscriptions or messages, add the following code before you create the `JmsConnectionFactory`:

    ``` js
      if (connection) {
        connectionStarted();
      } else {
        Tracer.setTrace(true); // enable logging
        var factory = new JmsConnectionFactory(url);
    ```

    Then open the browser’s console to see the logged messages. (This only works on browsers that implement `console.log`.)

### Related documentation

[JavaScript JMS Client API](http://developer.kaazing.com/documentation/jms/4.0/apidoc/client/javascript/jms/index.md)

Notes
-----

-   Clients built using KAAZING Gateway 3.x libraries will work against KAAZING Gateway 4.x. If you wish to upgrade your 3.x client to the 4.x libraries, please note that the 3.x clients used a single JMS library and 4.x clients include and use separate WebSocket and JMS libraries. Update your client library file and code references to include both the WebSocket and JMS libraries, as described in the 4.x documentation.
-   `TemporaryTopic` and `TemporaryQueue` objects are destroyed when the client loses its connection to the Gateway, or when the JMS-compliant message broker loses its connection to the Gateway. To address this, monitor the client's exception listener to handle recovery for your application. Once the connection is re-established, recreate `TemporaryTopic` and `TemporaryQueue`. `ConnectionDroppedException` and `ConnectionInterruptedException` are delivered to the connection's exception listener via `onException`, indicating that messages in flight might be lost, depending on message delivery options. `ConnectionRestoredException` is delivered to indicate that the connection through to the JMS-compliant message broker has been re-established. `TemporaryTopic` and `TemporaryQueue` should be recreated at that time to resume operations.

Summary
-------

The Javascript JMS client libraries closely match the server-side JMS libraries. For more information about the KAAZING Gateway JavaScript JMS Client API, see [KAAZING Gateway JMS Client Libraries: Supported APIs](../about/kaazing-jms-api.md).


