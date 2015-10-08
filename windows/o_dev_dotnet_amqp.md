Build Microsoft .NET and Silverlight AMQP Clients  ![This feature is available in KAAZING Gateway - Enterprise Edition](images/enterprise-feature.png)
====================================================================

This checklist provides the steps necessary to use the signed KAAZING Gateway AMQP client libraries for Microsoft .NET and Silverlight to enable your .NET Framework or Silverlight application to communicate with any AMQP broker:

| \# | Step                                                                                                                                | Topic or Reference                                                                                                                                       |
|:---|:------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1  | Learn about the KAAZING Gateway AMQP client.                                                                                        | [Overview of the KAAZING Gateway .NET and Silverlight AMQP Client Libraries](#overview-of-the-kaazing-gateway-net-and-silverlight-amqp-client-libraries) |
| 2  | Learn how to use the Gateway AMQP Client Library and the supported APIs.                                                            | [Use the KAAZING Gateway .NET and Silverlight AMQP Client Library](#use-the-kaazing-gateway-net-and-silverlight-amqp-client-library)                     |
| 3  | Learn how to authenticate your client by implementing a challenge handler to respond to authentication challenges from the Gateway. | [Secure Your .NET and Silverlight AMQP Client](#secure-your-microsoft-net-and-silverlight-client)                                                        |
| 4  | Migrate your KAAZING Gateway 4.x clients to KAAZING Gateway 5.0.x.                                                                  | [Migrate Microsoft .NET and Silverlight Applications to KAAZING Gateway 5.0](#migrate-microsoft-net-and-silverlight-applications-to-kaazing-gateway-50x) |

Overview of AMQP 0-9-1
----------------------

Advanced Message Queuing Protocol (AMQP) is an open standard for messaging middleware that was originally designed by the financial services industry to provide an interoperable protocol for managing the flow of enterprise messages. To guarantee messaging interoperability, AMQP 0-9-1 defines both a wire-level protocol and a model—the AMQP Model—of messaging capabilities.

The AMQP Model defines three main components:

1.  *Exchange*: clients publish messages to an exchange
2.  *Queue*: clients read messages from a queue
3.  *Binding*: a mapping from an exchange to a queue

An AMQP client connects to an AMQP broker and opens a *channel*. Once the channel is established, the client can send messages to an exchange and receive messages from a queue. To learn more about AMQP functionality, take a look at the [Real-Time Interactive Guide to AMQP](../guide-amqp.md), an interactive guide that takes you step-by-step through the main features of AMQP version 0-9-1.

For more information about AMQP, visit [http://www.amqp.org](http://www.amqp.org).

Overview of Microsoft .NET Framework
------------------------------------

Microsoft .NET Framework (.NET) provides a common language runtime, base libraries, and development technologies to build applications for Microsoft Windows desktop, mobile and server platforms.

For more information about the .NET Framework, visit [http://www.microsoft.com/NET/](http://www.microsoft.com/NET/).

Overview of Microsoft Silverlight
---------------------------------

Microsoft Silverlight (Silverlight) is a browser plugin that enables rich Internet applications. It runs in browsers on the Microsoft Windows and Mac operating systems. For more information about Silverlight, visit [http://www.microsoft.com/silverlight](http://www.microsoft.com/silverlight).

WebSocket and AMQP
------------------

WebSocket enables direct communication from the browser to an AMQP broker. KAAZING Gateway radically simplifies Web application design by providing the AMQP client libraries for the Java, JavaScript, Adobe Flex, .NET and Silverlight client technologies. Web developers can code directly against the back-end AMQP broker without the need for custom Servlets or server-side programming.

Using the AMQP client libraries, you can take advantage of the AMQP features, making the browser a first-class citizen in AMQP systems (similar to C, Java, Python, and other clients). This means that you can run AMQP clients directly in a browser.

The AMQP libraries use the KAAZING Gateway ByteSocket client library because AMQP messages use a binary format. The implementation is layered on top of ByteSocket, which uses WebSocket (and thus the entire stack of KAAZING Gateway HTML5 Communications client libraries). As such, applications developed using the AMQP libraries are provided with guaranteed persistence, reliability, and message-receipt acknowledgment all the way to the browser.

Overview of the KAAZING Gateway .NET and Silverlight AMQP Client Libraries
--------------------------------------------------------------------------

KAAZING Gateway - Community Edition includes AMQP client libraries, which allow clients to subscribe from and publish messages to a message broker using AMQP. With the KAAZING Gateway AMQP client libraries, you can leverage WebSocket in your application. This WebSocket client then enables communication over AMQP between your application and the message broker, as shown in the following figure:

![Kaazing WebSocket Gateway Flash AMQP Client](images/f-amqp-web-silverlight-client-web.jpg)

Starting an AMQP Broker
-----------------------

There are a wide variety of AMQP brokers available that implement different AMQP versions. For example, RabbitMQ, Apache Qpid, OpenAMQ, Red Hat Enterprise MRG, ØMQ, and Zyre. If you do not have an AMQP broker installed yet, you can use the Apache Qpid AMQP broker included in the Gateway download package, that supports AMQP version 0-9-1.

To set up and start the Apache Qpid broker on your system, perform the steps described in [Setting Up KAAZING Gateway](../about/setup-guide.md).

**Note**: The AMQP client libraries are compatible with AMQP version 0-9-1. Refer your AMQP broker documentation for information about certified AMQP versions.

For information on integrating with RabbitMQ, see [Integrate RabbitMQ Messaging](../integration-amqp/p_amqp_integrate_rabbitmq.md).

Taking a Look at the AMQP Demo
------------------------------

Before you start, take a look at the demonstrations built with the .NET and Silverlight versions of the AMQP client library. To see these demos in action, perform the following steps:

1.  Start KAAZING Gateway as described in [Setting Up KAAZING Gateway](../about/setup-guide.md).
2.  In a browser, navigate to [http://localhost:8001/demo/amqp/silverlight/?d=amqp-silverlight](http://localhost:8001/demo/amqp/silverlight/?d=amqp-silverlight) and [http://localhost:8001/demo/amqp/dotnet/](http://localhost:8001/demo/amqp/dotnet/).
     From this page, you can navigate to additional AMQP demos for different client technologies (Java, JavaScript, and Adobe Flash and Flex).

Use the KAAZING Gateway .NET and Silverlight AMQP Client Library
================================================================

In this procedure, you will learn how to use the signed KAAZING Gateway .NET and Silverlight AMQP Client Library and the supported APIs.

To Use the KAAZING Gateway .NET and Silverlight AMQP Client Library
-------------------------------------------------------------------

1.  Setting up your .NET and Silverlight development environment.

   To develop applications for Silverlight or the .NET Framework, you must install the a .NET Integrated Development Environment (IDE) such as Visual Studio or the free Visual Studio Express.

   **Note**: You can develop Silverlight and .NET Framework applications in any of the .NET programming languages. Microsoft Visual C\# is used in this how-to.

   1.  Set up your .NET Framework environment.

       In addition to the Visual Studio IDE, .NET Framework application development also requires:

       -   A .NET Integrated Development Environment (IDE), such as Visual Studio or the free Visual Web Developer Express
       -   Microsoft .NET Framework 4 with Microsoft .NET Framework 4 Patch KB2468871 (download the patch from <http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=3556>). This is required for building or running .NET clients using the KAAZING Gateway .NET Client Library.

       To use the KAAZING Gateway AMQP client libraries for .NET and Silverlight, you must add a reference to the libraries located in the `.DLL` files `Kaazing.AMQP.dll` and `Kaazing.Gateway.dll` to your .NET application. This file is in the directory `GATEWAY_HOME/lib/client/dotnet/Release`. Additional `.DLL`s may be required to program with other protocols.

       Applications based on the .NET Framework can use standard .NET Forms Controls or Windows Presentation Foundation (WPF). Note that there are slight differences in APIs for Controls, Threading and Dispatching behavior. The examples below illustrate event dispatching for WPF applications. WPF Applications, like Silverlight applications, consist of Extensible Application Markup Language (XAML) files and their corresponding code-behind classes `.XAML.CS` (in C\#).

       .NET Framework applications are typically deployed as an executable `.EXE` file. This executable references client library `.DLL` files that can be deployed in the same directory alongside the executable, or registered through an installation process.

   2.  Set up your Silverlight environment.

       In addition to the Visual Studio IDE, Silverlight application development also requires:

       -   A .NET Integrated Development Environment (IDE), such as Visual Studio or the free Visual Web Developer Express
       -   Microsoft Silverlight 4 SDK
       -   The Silverlight 4 browser plugin

       To use the KAAZING Gateway AMQP client libraries for .NET and Silverlight, you must add a reference to the libraries located in the `.DLL` files `Kaazing.AMQP.dll` and `Kaazing.Gateway.dll` to your Silverlight project. This file is in the directory `GATEWAY_HOME/lib/client/dotnet/Release`.

       **Note**: You can develop Silverlight 4 applications in any of the .NET programming languages. Microsoft Visual C\# is used in this how-to.

       Silverlight applications primarily consist of Extensible Application Markup Language (XAML) files and their corresponding code-behind classes `.XAML.CS` (in C\#). The `.XAML` files typically contain the User Interface (UI) elements and the `.XAML.CS` files contain the page's code-behind classes.

       A typical Silverlight application is deployed as a compressed `.XAP` file. Adding the reference to the Silverlight client library `.DLL` file in your Silverlight application packages the `.DLL` file in your finished application. To deploy your Silverlight application `.XAP` file, you can host it on KAAZING Gateway. For example, you can place it in the web directory: `GATEWAY_HOME/web/base`.

2.  Configure KAAZING Gateway to connect to an AMQP broker.

     **Note:** If you have KAAZING Gateway running on `localhost` and if you have an AMQP broker running on `localhost` at the default AMQP port `5672`, you do not have to configure anything to see the AMQP demos and the interactive AMQP guide.

     The following is an example of the default configuration element for the AMQP service in the KAAZING Gateway bundle, as specified in the configuration file `GATEWAY_HOME/conf/gateway-config.xml`:

     ``` xml
       <service>
               <accept>ws://localhost:8001/amqp</accept>
               <connect>tcp://localhost:5672</connect>

               <type>amqp.proxy</type>

               <!--
               <authorization-constraint>
                       <require-role>AUTHORIZED</require-role>
               </authorization-constraint>
               -->

               <cross-site-constraint>
                       <allow-origin>http://localhost:8001</allow-origin>
               </cross-site-constraint>
       </service>
     ```

     In this case, the service is configured to accept WebSocket AMQP requests from the browser at `ws://localhost:8001/amqp` and proxy those requests to a locally installed AMQP broker (`localhost`) at port `5672`.

     To configure the Gateway to accept WebSocket requests at another URL or to connect to a different AMQP broker, you can edit `GATEWAY_HOME/conf/gateway-config.xml`, update the values for the `accept` elements, change the `connect` property, and restart the Gateway. For example, the following configuration configures KAAZING Gateway to accept WebSocket AMQP requests at `ws://www.example.com:80/amqp` and proxy those requests to an AMQP broker (`amqp.example.com`) on port `5672`.

     ``` auto-links:
     <service>
         <accept>ws://www.example.com:80/amqp</accept>
         <connect>tcp://amqp.example.com:5672</connect>

         <type>amqp.proxy</type>
     </service>
     ```

3.  Setting up an AMQP broker.

     **Note**: The KAAZING Gateway AMQP client libraries are compatible with AMQP version 0-9-1. Refer your AMQP broker documentation for information about supported AMQP versions.

     There are a wide variety of AMQP brokers available that implement different AMQP versions. For example, RabbitMQ, Apache Qpid, OpenAMQ, Red Hat Enterprise MRG, ZeroMQ, and Zyre. If you do not have an AMQP broker installed yet, you can use Apache Qpid AMQP broker, which supports AMQP version 0-9-1. To set up the Apache Qpid broker on your system, perform the steps described in [Setting Up KAAZING Gateway](../about/setup-guide.md), open a browser, navigate to [`http://localhost:8001/demo/amqp/silverlight/?d=amqp-silverlight`](http://localhost:8001/demo/amqp/silverlight/?d=amqp-silverlight)[](http://localhost:8001/demo/amqp/silverlight/?d=amqp-silverlight), and log in to the demo to verify that it is working.

4.  Review the common .NET and Silverlight AMQP programming steps.

     Now that you have set up your environment to develop .NET and Silverlight applications using the Gateway's AMQP client library, you can start creating your application. You can either build a single application that both publishes and consumes messages, or create two different applications to handle each action. The demo located at <http://localhost:8001/demo/amqp/silverlight/?d=amqp-silverlight> shows a single application that handles both actions. You can view the source code for this demo in `GATEWAY_HOME/demo/dotnet/src/amqp`. Refer to the [Kaazing.AMQP](http://developer.kaazing.com/documentation/5.0/apidoc/client/dotnet/html/N_Kaazing_AMQP.htm) documentation for the complete list of all the AMQP command and callback functions.

5.  Create the AmqpClient object.

     First, create an AmqpClient client object. Before you create the client object, add the following import statements in your application page's `.XAML.CS` file:

     ``` cs
     using com.kaazing.gateway.client;
     using Kaazing.AMQP;
     ```

     Add the following member variables:

     ``` cs
     private AmqpChannel     publishChannel = null;
     private AmqpChannel     consumeChannel = null;
     private string  exchangeName = "demo_exchange";
     private string  queueName = "myqueuename";
     private string  routingKey = "broadcastkey";
     private string  amqpVersion = "0-9-1";
     private bool    passive = false;
     private bool    durable = false;
     private bool    noWait = false;
     private bool    exclusive = false;
     private bool    autoDelete = false;
     private bool    mandatory = false;
     private bool    immediate = false;
     private bool    noLocal = false;
     private bool    noAck = false;
     ```

     Next, declare a variable and create an instance of the `AmqpClient` object as shown in the following example.

     ``` cs
     private AmqpClient client = new AmqpClient();
     ```

     Now that you have created an instance of the `AmqpClient` object, you can use the AMQP protocol commands. To handle open and close events, add two event handlers—one for `OpenEvent` and one for `CloseEvent`, as shown in the following example.

     ``` cs
     client.OpenEvent += new AmqpEventHandler(OpenHandler);
     client.CloseEvent += new AmqpEventHandler(CloseHandler);
     ```

6.  Connect to an AMQP broker.

     Next, you must connect and log in to an AMQP broker. <span class="stockTable">The client generally manages all of its communication on a single connection to an AMQP broker. You establish a connection to an AMQP broker by passing in the broker address, a user name and password, the AMQP version you want to use, and, optionally, a virtual host name (the name of a collection of exchanges and queues hosted on independent server domains).</span> In the following example, the parameters are passed in when you call the `connect()` method.

     ``` cs
     client.Connect(url, virtualHost, username, password);
     ```

     In this example, the parameters that are passed in may be: url: `ws://localhost:8001/amqp`, virtualHost: `/`, username: `guest`, and password: `guest`.

     **Note**: The Gateway supports AMQP version 0-9-1.

7.  Create channels.

     Once a connection to an AMQP broker has been established, the client must create a channel to communicate to the broker. A channel is a bi-directional connection between an AMQP client and an AMQP broker. AMQP is multi-channeled, which means that channels are multiplexed over a single network socket connection. Channels are light-weight and consume little resources, and therefore used in AMQP's exception handling mechanism—channels are closed when an exception occurs. The following example shows how you can create two channels (one for publishing to an exchange and one for consuming from a queue):

     ``` cs
     publishChannel = client.OpenChannel();
     consumeChannel = client.OpenChannel();
     ```

     Once you have created the channels, you can add event handlers for various channel events as shown in the following example.

     ``` cs
     publishChannel.OpenEvent += new AmqpEventHandler(PublishOpenHandler);
     publishChannel.CloseEvent += new AmqpEventHandler(PublishCloseChannelHandler);
     publishChannel.DeclareExchangeEvent += new AmqpEventHandler(DeclareExchangeHandler);

     consumeChannel.OpenEvent += new AmqpEventHandler(ConsumeOpenHandler);
     consumeChannel.CloseEvent += new AmqpEventHandler(ConsumeCloseChannelHandler);
     consumeChannel.ConsumeEvent += new AmqpEventHandler(ConsumeHandler);
     consumeChannel.BindQueueEvent += new AmqpEventHandler(BindQueueHandler);
     consumeChannel.DeclareQueueEvent += new  AmqpEventHandler(DeclareQueueHandler);
     consumeChannel.FlowEvent += new AmqpEventHandler(FlowHandler);
     consumeChannel.MessageEvent += new AmqpEventHandler(MessageHandler);
     ```

     In this example, each of the event handlers has an associated function that processes the AMQP event. The following is an example of a `PublishOpenHandler` function:

     ``` cs
     private void PublishOpenHandler(object sender, AmqpEventArgs e){
         this.Dispatcher.BeginInvoke(() =>
         {
                 Log("OPENED: Publish Channel");
         });
     }
     ```

8.  Declare an exchange.

     AMQP messages are published to exchanges. Messages contain a *routing key* that contains the information about the message's destination. The exchange accepts messages and their routing keys and delivers them to a message queue. You can think of an exchange as an electronic mailman that delivers the messages to a mailbox (the queue) based on the address on the message's envelope (the routing key). Exchanges do not store messages.

     **Note**: AMQP brokers reserve the use of the `System` exchange type, and thus should not be used by applications.

     AMQP defines different exchange types. Some of these exchange types (Direct, Fanout, and Topic) must be supported by all AMQP brokers while others (Headers and System) are optional. AMQP brokers can also support custom exchange types. The following are the different types of exchanges:

     -   **Direct**—Messages are sent only to a queue that is bound with a binding key that matches the message's routing key.
     -   **Fanout**—Messages are sent to every queue that is bound to the exchange.
     -   **Topic**—Messages are sent to a queue based on categorical binding keys and wildcards.
     -   **Headers**—Messages are sent to a queue based on their header property values.
     -   **System**—Messages are sent to system services.

     Exchanges can be *durable*, meaning that the exchange survives broker shut-down and must be deleted manually or *non-durable* (temporary) meaning that the exchange lasts only until the broker is shut down. Finally, to check if an exchange exists on the AMQP broker (without actually creating it), you can create a *passive* exchange. The following example shows how you can create a direct exchange on the publish channel:

     ``` cs
     private void DeclareExchange()
     {
         publishChannel.DeclareExchange(exchangeName, "fanout", passive, durable, noWait, null);
     }
     ```

     **Note**: In this example, the arguments `passive`, `durable`, and `noWait` represent boolean values. Note also that no custom parameters are passed in.

     After the exchange is created successfully, a `DeclareExchangeEvent` event is raised, which calls the previously registered event handler `DeclareExchangeHandler`.

9.  Declare a queue.

     AMQP messages are consumed from queues. You can think of a queue as a mailbox; messages addressed to a particular address (the routing key) are placed in the mailbox for the consumer to pick up. If multiple consumers are bound to a single queue, only one of the consumers receives the message (the one that picked up the mail).

     To check if a queue exists on the AMQP broker (without creating it), you can create a *passive* queue. Additionally, queues can be marked *exclusive,* which means that they are tied to a specific connection. If a queue is marked exclusive, it is deleted when the connection on which it was created is closed.

     Queues can be *durable*, meaning that the queue survives broker shut-down and must be deleted manually or *non-durable* (temporary) meaning that the queue lasts only until the broker is shut down. Queues can also be marked *auto delete*, which means that the queue is automatically deleted when it is no longer in use. The following example shows how you can create a queue on the consume channel:

     ``` cs
     consumeChannel.DeclareQueue(queueName, passive, durable, exclusive, autoDelete, noWait, null);
     ```

     **Note**: In this example, the arguments `passive`, `durable`, `exclusive`, `autoDelete`, and `noWait` represent boolean values. Note also that no custom parameters are passed in.

     After the queue is created successfully, a `DeclareQueueEvent` event is raised, which calls the previously registered event handler `DeclareQueueHandler`.

10. Bind an exchange to a queue.

     Once you have created an exchange and a queue in AMQP, you must bind—or map—one to the other so that messages published to a specific exchange are delivered to a particular queue. You bind a queue to an exchange with a routing key as shown in the following example.

     ``` cs
     consumeChannel.BindQueue(queueName, exchangeName, routingKey, noWait, null);
     ```

     After the exchange is bound to the queue successfully, a `BindQueueEvent` event is raised, which calls the previously registered event handler `BindQueueHandler`.

11. Publish messages.

     Messages are published to exchanges. The established binding rules (routing keys) determine to which queue a message is delivered. Messages have content that consists of two parts:

     1.  **Content Header**—A set of properties that describes the message
     2.  **Content Body**—A blob of binary data

     Additionally, messages can be marked *mandatory* to send a notification to the publisher in case a message cannot be delivered to a queue. You can also mark a message *immediate* so that it is returned to the sender if the message cannot be routed to a queue consumer immediately. The following example shows how the content body of a message is added to a buffer (AMQP uses a binary message format) and published to an exchange using the publish channel:

     ``` cs
     private void PublishBasic(string text)
         {
             ByteBuffer buffer = new ByteBuffer();
             buffer.PutString(text, System.Text.Encoding.UTF8);
             buffer.Flip();
             AmqpProperties amqpProperties = new AmqpProperties();
             amqpProperties.MessageId = "abcdxyz1234pqr";
             amqpProperties.CorrelationId = "23456";
             amqpProperties.UserId = "guest";
             amqpProperties.ContentType = AmqpProperties.TEXT_PLAIN;
             amqpProperties.DeliveryMode = 1;
             amqpProperties.Priority = 6;
             amqpProperties.Timestamp = DateTime.Now.ToLocalTime();
             AmqpArguments customHeaders = new AmqpArguments();
             customHeaders.AddInteger("KZNG_AMQP_KEY1", 100);
             customHeaders.AddLongString("KZNG_AMQP_KEY2", "Custom Header Value");
             amqpProperties.Headers = customHeaders;
             publishChannel.PublishBasic(buffer, amqpProperties, exchangeName,
                     routingKey, false, false);
             Log(amqpProperties.ToString());
             Log("Published Message Properties:");
             Log("MESSAGE PUBLISHED: " + text);
         }
     ```

     The `AmqpProperties` class defines pre-defined properties as per AMQP 0-9-1 spec and provides type-safe getters and setters for those pre-defined properties. The value of AMQP 0-9-1's standard "headers" property is of type [AmqpArguments](http://developer.kaazing.com/documentation/5.0/apidoc/client/dotnet/html/N_Kaazing_AMQP.htm). The KAAZING Gateway AMQP implementation uses AmqpArguments to encode the table. Similarly, the KAAZING Gateway AMQP implementation decodes the table and constructs an instance of AmqpArguments.

     The arguments `mandatory` and `immediate` use boolean values. Note also that no custom parameters are passed in.

     The username set with the `UserId` method must match the user that is currently authenticated with the AMQP broker. If they do not match you will see the following error:
     `PRECONDITION_FAILED - user_id property set to '<name>' but authenticated user was '<name>'`

12. Consume messages.

     Once messages are published, they can be consumed from a queue. A variety of options can be applied to messages in a queue. For example, publishers can choose to require acknowledgement (*ack*) of messages so that messages can be redelivered in the case of a delivery failure. If the queue is set to *exclusive*, it is scoped to just the current connection and deleted when the connection on which it was established is closed. Additionally, you can use the *no local* setting to notify the broker *not* to send messages to the connection on which the messages were published. The following example shows how you can consume messages from a queue on the consume channel:

     ``` cs
     consumeChannel.DeclareQueue(queueName, false, false, false, false, false, null)
         .BindQueue(queueName, exchangeName, routingKey, false, null)
         .ConsumeBasic(queueName, routingKey, false, false, false, false, null);
     ```

     **Note**: In this example, the arguments `noLocal`, `noAck`, `noWait`, and `exclusive` represent boolean values.

     After the `ConsumeBasic()` method is successful, a `ConsumeEvent` event is raised, which calls the previously registered event handler `ConsumeHandler`. The AMQP broker can then start delivering messages to the client and these messages raise the `MessageEvent` event, which calls the previously registered event handler `MessageHandler`. The following example shows how the `MessageHandler` function retrieves information from the `AmqpEventArgs` object.

     ``` cs
     private void MessageHandler(object sender, AmqpEventArgs e)
     {
         this.BeginInvoke((InvokeDelegate)(() =>
         {
             ByteBuffer buf = e.Body;
             string message = buf.GetString(System.Text.Encoding.UTF8);
             Log(e.AmqpProperties.ToString());
             Log("Consumed Message Properties:");
             Log("MESSAGE CONSUMED: " + message);

             // Explicitly acknowledge the message as we are passing
             // 'false' as the value of the 'noAck' parameter in
             // the consumeChannel.ConsumeBasic() call.
             long dt = Convert.ToInt64(e.Arguments["deliveryTag"]);
             ((AmqpChannel)sender).AckBasic(dt, true);
         }));
     }
     ```

     Here you can see how the properties are retrieved using the AmqpProperties method. A method from the same AmqpProperties class used to encode the properties of the published message.

     ##### Message Acknowledgement

     The Boolean parameter `noAck` is optional with the default value of `true`. If `noAck` is `true`, the AMQP broker will not expect any acknowledgement from the client before discarding the message. If `noAck` is `false`, then the AMQP broker will expect an acknowledgement before discarding the message. If `noAck` is specified to be `false`, then you must explicitly acknowledge the received message using `AmqpChannel` `ackBasic()`.

     In the AMQP demo code in this procedure, message acknowledgement is being performed because `false` was passed in for `noAck` in `ConsumeBasic()`. If the client acknowledges a message **and** `noAck` is `true` (the default setting), then the AMQP message broker will close the channel.

13. Use transactions.

     AMQP supports transactional messaging, through *server local transactions*. In a transaction, the server only publishes a set of messages as one unit when the client commits the transaction. Transactions only apply to message publishing and not to the consumption of the messages.

     **Note**: Once you commit or rollback a transaction on a channel, a new transaction is started automatically. For this reason you must commit all future messages you want to publish on that channel or create a new, non-transactional channel on which to publish messages.

     The following transaction-related methods can be used to work select (start), commit, and rollback a transaction:

     ``` cs
     txnPublishChannel.SelectTx();
     txnPublishChannel.CommitTx();
     txnPublishChannel.RollbackTx();
     ```

     After the transaction is successfully selected, committed, or rolled back, the corresponding events (`SelectTransactionEvent`, `CommitTransactionEvent`, and `RollbackTransactionEvent`) are raised. These events call previously registered event handlers. Each of the event handlers has an associated function that processes the event.

     ``` cs
     txnPublishChannel.CommitTransactionEvent += CommitOkTxHandler;
     txnPublishChannel.RollbackTransactionEvent += RollbackOkTxHandler;
     txnPublishChannel.SelectTransactionEvent += SelectOkTxHandler;
     ```

     To see an example of transactions in an application, see the sample demo code in `GATEWAY_HOME/demo/silverlight/src/amqp/Page.xaml.cs`.

14. Control message flow.

     You can use flow control in AMQP to temporarily—or permanently—halt the flow of messages on a channel from a queue to a consumer. If you turn the message flow off, no messages are sent to the consumer. The following example shows how you can turn the flow of messages on a channel off and back on:

     ``` cs
     consumeChannel.FlowChannel(false);
     consumeChannel.FlowChannel(true);
     ```

     After the flow on a channel is halted or resumed successfully, a `FlowEvent` event is raised, which calls the previously registered event handler `FlowHandler`.

15. Handle exceptions.

     Channels are light-weight and cheap, and therefore used in AMQP's exception handling mechanism—channels are closed when an exception occurs. When the `CloseChannelEvent` event is raised, the previously registered `PublishCloseChannelHandler` event handler calls the associated `PublishCloseChannelHandler` function that processes the AMQP event. The following example shows how that function can be used to log a message about why the channel was closed:

     ``` cs
     private void PublishCloseChannelHandler(object sender, AmqpEventArgs args e)
     {
         this.Dispatcher.BeginInvoke(() =>
         {
             ByteBuffer buf = e.Body;
             string message = buf.GetString(System.Text.Encoding.UTF8);
             Log("MESSAGE: " + message);
             Log("Publish Channel Closed: " + message);
         });
     }
     ```

16. Add the Silverlight object tag.

     To ensure the Silverlight application works properly, you must add an `object` tag that points to the location of your XAP file to the HTML file that includes your Silverlight application. At runtime, Silverlight downloads the XAP file that is referenced and runs it in the browser. The following is an example `object` tag:

     ``` cs
     <object data="data:application/x-silverlight,"
         type="application/x-silverlight-2"
         width="100%" height="100%"
         id="MyApplicationId">

         <param name="source" value="MySilverlightApplication.xap"/>
         <param name="EnableHtmlAccess" value="true"/>

         <!--
         Optional parameters
         -->
     </object>
     ```

     The following attributes are required if you want to use KAAZING Gateway Silverlight client library in your Silverlight application.

     -   **source**—The path to the Silverlight application XAP file.
     -   **EnableHtmlAccess**—Enables the Kaazing Silverlight client library to communicate with the HTML page that contains it.

Other Coding Styles
-------------------

In this procedure, you have used an *event* programming style. You can also use a *continuation-passing* programming style. The following example shows how you can declare an exchange using the continuation-passing programming style:

`publishChannel.DeclareExchange(exchangeName, "direct", passive, durable, noWait, null, declareExchangeHandlerContinuation, errorHandlerContinuation);`

You can also combine the two programming styles.

Notes
-----

-   The Microsoft .NET 4.0 Framework has a maximum connection limit of two per domain, similar to the browser limitation. For any Microsoft .NET application that uses more than one WebSocket connection at a time, you must either ensure that any WebSocket connection is closed by using `WebSocket.Close()` before opening another WebSocket connection, or increase the connection limit on the application by updating the `maxconnection` attribute in the `app.config` file.
-   You can verify that Kaazing has signed the relevant .NET DLL by selecting the DLL in the File Browser, then right-clicking and opening the Properties dialog. On the Digital Signatures tab, you can view the Name of Signer value "Kaazing Corporation" and a timestamp of when the DLL was signed. The email address is "Not available." For more information, see [an example C program](http://msdn.microsoft.com/en-us/library/aa382384%28VS.85%29.aspx) that shows how to use the Microsoft mechanism to verify a signature (a DLL is one example of a Portable Executable, or PE, file). You can also learn more about [preventing DLL pre-loading attacks](http://support.microsoft.com/kb/2389418).

Secure Your Microsoft .NET and Silverlight Client
=================================================

Before you add security to your clients, follow the steps in [Checklist: Configure Authentication and Authorization](https://github.com/kaazing/gateway/blob/develop/doc/security/o_auth_configure.md) to set up security on KAAZING Gateway for your client. The authentication and authorization methods configured on the Gateway influence your client's security implementation. For information on secure network connections between clients and the Gateway, see [Checklist: Secure Network Traffic with the Gateway](https://github.com/kaazing/gateway/blob/develop/doc/security/o_tls.md).

To Secure Your Microsoft .NET and Silverlight Client
----------------------------------------------------

This section contains the following topics:

-   [Creating a Basic Challenge Handler](#creating-a-basic-challenge-handler)
-   [Managing Log In Attempts](#managing-log-in-attempts)
-   [Negotiate and Register a Location-Specific Challenge Handler](#negotiate-and-register-a-location-specific-challenge-handler)
-   [Using Wildcards to Match Sub-domains and Paths](#using-wildcards-to-match-sub-domains-and-paths)

Authenticating your client involves implementing a challenge handler to respond to authentication challenges from the KAAZING Gateway. If your challenge handler is responsible for obtaining user credentials, then you will also need to implement a login handler. For more information, see the [.NET and Silverlight Client API](http://developer.kaazing.com/documentation/5.0/apidoc/client/dotnet/gateway/html/N_Kaazing_HTML5.htm).

### Creating a Basic Challenge Handler

A challenge handler is a constructor used in an application to respond to authentication challenges from the KAAZING Gateway when the application attempts to access a protected resource. Each of the resources protected by the KAAZING Gateway is configured with a different authentication scheme (for example, `Basic`, `Application Basic`, or `Application Token`), and your application requires a challenge handler for each of the schemes that it will encounter or a single challenge handler that will respond to all challenges. Also, you can add a dispatch challenge handler to route challenges to specific challenge handlers according to the URI of the requested resource.

For information about each authentication scheme type, see [Configure the HTTP Challenge Scheme](https://github.com/kaazing/gateway/blob/develop/doc/security/p_authentication_config_http_challenge_scheme.md).

Clients with a single challenge handling strategy for all 401 challenges can simply set a specific challenge handler as the default using `ChallengeHandlers.Default`. The following is an example of how to implement a single challenge handler for all challenges:

```

public partial class HTML5DemoForm : Form
{
  private WebSocket webSocket = null;
  private WebSocketFactory factory = new WebSocketFactory();
  ...
  BasicChallengeHandler basicHandler = BasicChallengeHandler.Create();
  basicHandler.LoginHandler = new LoginHandlerDemo(this);
  ...
  factory.ChallengeHandler = basicHandler;
  webSocket = factory.CreateWebSocket();
...
```

### Managing Log In Attempts

When it is not possible for the KAAZING Gateway client to create a challenge response, the client must return `null` to the Gateway to stop the Gateway from continuing to issue authentication challenges.

The following .NET example demonstrates how to stop the Gateway from issuing further challenges.

```
public delegate void InvokeDelegate();

private const int maxRetries = 2;    // max retry number on wrong credentials
private int retry = 0;               // number of retry

/// <summary>
/// .Net HTML5 Demo Form
/// </summary>

public PasswordAuthentication AuthenticationHandler()
{
  PasswordAuthentication credentials = null;
  if (retry++ >= maxRetries)
  {
    return null;          // stop authentication when max retries reached
  }
  AutoResetEvent userInputCompleted = new AutoResetEvent(false);
  this.BeginInvoke((InvokeDelegate)(() =>
  {
    LoginDemoForm loginForm = new LoginDemoForm();
    if (loginForm.ShowDialog() == System.Windows.Forms.DialogResult.OK)
    {
      credentials = new PasswordAuthentication(loginForm.Username, loginForm.Password.ToCharArray());
    }
    else
    {
      //user click cancel button to stop the authentication
      retry = 0;     // reset retry counter
      credentials = null;  // return null to stop authentication process
    }
    userInputCompleted.Set();
  }));
  // wait user click 'OK' or 'Cancel' on login window
  this.BeginInvoke((InvokeDelegate)(() =>
  {
    Log("CONNECTED");

    retry = 0;     // reset retry counter
    DisconnectButton.Enabled = true;
    SendButton.Enabled = true;
  }));
  {
    Log("DISCONNECTED");

    retry = 0;     // reset retry counter
    ConnectButton.Enabled = true;
    DisconnectButton.Enabled = false;
    SendButton.Enabled = false;
    userInputCompleted.WaitOne();
    return credentials;
```

### Negotiate and Register a Location-Specific Challenge Handler

Client applications with location-specific challenge handling strategies can register a `DispatchChallengeHandler` object, on which location-specific `ChallengeHandler` objects are then registered. The result is that whenever a request matching one of the specific locations encounters a 401 challenge from the server, the corresponding `ChallengeHandler` object is invoked to handle the challenge.

The following example creates a challenge handler to handle authentications and uses `DispatchChallengeHandler` to assign different challenge handler to each location:

```
DispatchChallengeHandler dispatchHandler = DispatchChallengeHandler.Create();
LoginHandler loginHandler = new LoginHandlerDemo(this);
```

Next, set a `loginHandler` for this location:

```
BasicChallengeHandler basicHandler = BasicChallengeHandler.Create();
basicHandler.LoginHandler = loginHandler;
dispatchHandler.Register("ws://myserver.com/*", basicHandler);
```

Add another challenge handler for a second location:

```
BasicChallengeHandler basicHandler2 = BasicChallengeHandler.Create();
basicHandler2.LoginHandler = loginHandler2;
dispatchHandler.Register("ws://otherserver.com/*", basicHandler2);
factory.ChallengeHandler = dispatchHandler;
```

### Using Wildcards to Match Sub-domains and Paths

You can use wildcards (“\*”) when registering locations using `DispatchChallengeHandler`. Some examples of `locationDescription` values with wildcards are:

-   `*/` matches all requests to any host on port 80 (default port), with no user information or path specified.
-   `*.hostname.com:8000` matches all requests to port 8000 on any sub-domain of hostname.com, but not hostname.com itself.
-   `server.hostname.com:*/*` matches all requests to a particular server on any port on any path but not the empty path.

See Also
--------

[.NET and Silverlight Client API](http://developer.kaazing.com/documentation/5.0/apidoc/client/dotnet/gateway/html/N_Kaazing_HTML5.htm)


Migrate Microsoft .NET and Silverlight Applications to KAAZING Gateway 5.0.x
============================================================================

This topic explains how to migrate KAAZING Gateway 4.x Microsoft .NET and Silverlight clients to KAAZING Gateway 5.0.x.

**Notes:**

  -   The KAAZING Gateway Microsoft .NET and Silverlight library supports all platforms where Microsoft .NET Framework 4.5 is supported. To use the full RFC-6455 support in the KAAZING Gateway Microsoft .NET and Silverlight library requires Windows 8 or above.

To Migrate ByteSocket Applications to KAAZING Gateway 5.0.x
--------------------------------------------------------------

There are two issues when migrating to the KAAZING Gateway 5.0.x Microsoft .NET and Silverlight library:

1. To use the KAAZING Gateway 5.0.x Microsoft .NET and Silverlight library, use the NuGet package that contains all of the DLLs needed for all of the supported deployment environments. Starting with Visual Studio 2012, NuGet is included in every edition (except Team Foundation Server) by default.
    1. Install the [NuGet Command-Line Utility](https://docs.nuget.org/consume/installing-nuget).
    2. From the command-line, navigate to the library folder containing **Kaazing.WebSocket.nuspec**.
    2. Run the command, `nuget pack Kaazing.WebSocket.nuspec`. This will create the file **Kaazing.WebSocket.nupack**. Note the location of this file.
    3. Open the Visual Studio project for your client application.
    4. Click **Tools**, and then click **NuGet Package Manager**, and then **Package Manager Settings**.
    5. Click **Package Sources**.
    6. Click the plus sign (+) to add a new source.
    7. For the **Name**, enter **KAAZING**, and for **Source**, click **...** and navigate to the folder containing the file **Kaazing.WebSocket.nupack** and click **OK**.
    8. Click **Update** and then click **OK**.
    9. Click **Tools**, and then click **NuGet Package Manager**, and then **Manage NuGet Packages for Solution**.
    10. Click **All**, then click **KAAZING**. The **Kaazing.WebSocket** library appears.
    11. Click **Install**.
    12. In **Select Project**, select your project and click **OK**. The KAAZING Gateway Microsoft .NET and Silverlight client library is added to the project.
    13. To see the library package, in **Solution Explorer**, expand the **References** section. **Kaazing.WebSocket** is listed. You can double-click the package to see it in the **Object Explorer**.

2. Change the scope of the client authentication Challenge Handler code from global to connection-level. In KAAZING Gateway 4.x, the Challenge Handler scope was set globally. For example:

    ```
    //Set up ChallengeHandler to handle Basic/Application Basic authentications
    BasicChallengeHandler basicHandler = ChallengeHandlers.Load<BasicChallengeHandler>(typeof(BasicChallengeHandler));
    basicHandler.LoginHandler = new LoginHandlerDemo(this);
    ChallengeHandlers.Default = basicHandler;
    ```

    In KAAZING Gateway 5.0.x, the Challenge Handler scope is set at the connection level, using `WebSocketFactory()`. For example:

    ```
    factory = new WebSocketFactory();
    BasicChallengeHandler handler = BasicChallengeHandler.Create();
    handler.LoginHandler = new LoginHandlerDemo(this);
    factory.ChallengeHandler = handler;
    ```

See Also
--------

[.NET and Silverlight Client API](http://developer.kaazing.com/documentation/5.0/apidoc/client/dotnet/gateway/html/N_Kaazing_HTML5.htm)
