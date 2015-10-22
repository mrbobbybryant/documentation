Troubleshoot JMS Integration  ![This feature is available in KAAZING Gateway - Enterprise Edition](../images/enterprise-feature.png)
============================

This topic covers troubleshooting issues related to integrating a JMS-compliant message broker with KAAZING Gateway.

For information on troubleshooting JMS clients, see [Troubleshoot Your Clients](../troubleshooting/p_dev_troubleshoot.md).

For general troubleshooting information, see [Troubleshoot KAAZING Gateway](../troubleshooting/o_troubleshoot.md).

What Problem Are You Having?
----------------------------

-   [Exception: "Name Not Found" When Using TIBCO EMS](#exception-name-not-found-when-using-tibco-ems)
-   [Exception: "No Initial Context Exception" When Using TIBCO EMS](#exception-no-initial-context-exception-when-using-tibco-ems)
-   [Exception: Service Unavailable When Using TIBCO EMS](#exception-service-unavailable-when-using-tibco-ems)
-   [Connection Errors](#connection-errors)
-   [Exception: A synchronous method call is not permitted](#exception-a-synchronous-method-call-is-not-permitted)
-   [An Authentication Error Occurs when Trying to Connect the Gateway to the Message Broker](#an-authentication-error-occurs-when-trying-to-connect-the-gateway-to-the-message-broker)
-   [Exception: ClassNotFoundException](#exception-classnotfoundexception)
-   [TIBCO EMS Unable to Free Up Durable Subscribers](#tibco-ems-unable-to-free-up-durable-subscribers)
-   [Warning: maximum.pending.acknowledgments has been set to 1](#warning-maximumpendingacknowledgments-has-been-set-to-1)

Exception: "Name Not Found" When Using TIBCO EMS
-----------------------------------------------------------------------

**Cause:** When you try to run a client application against TIBCO EMS 5.x, you get an exception that looks similar to the following:

`javax.naming.NameNotFoundException`

The `TIBCO_HOME/ems/5.1/bin/factories.conf` file may not be correctly configured.

**Solution:** To prevent this error, ensure that the `TIBCO_HOME/ems/5.1/bin/factories.conf` file is correctly configured according to the TIBCO EMS 5.x documentation. You can alternatively copy the contents of the file from the TIBCO EMS 5.x samples (`TIBCO_HOME/ems/5.1/samples/config`) into the `factories.conf` file.

Exception: "No Initial Context Exception" When Using TIBCO EMS
-------------------------------------------------------------------------------------

**Cause:** When you try to start the Gateway, you get an exception that looks similar to the following:

`javax.naming.NoInitialContextException`

This error may occur if you do not have the necessary TIBCO EMS JAR files in your *GATEWAY\_HOME*.

**Solution:** To prevent this error, copy the following client JAR files from `TIBCO_HOME/ems/version/lib` to `GATEWAY_HOME/lib`:

-   `jms.jar`
-   `tibcrypt.jar`
-   `tibjms.jar`

Then, restart the Gateway.

Exception: Service Unavailable When Using TIBCO EMS
---------------------------------------------------

**Cause:** After starting the Gateway, you see the following error:

`javax.naming.ServiceUnavailableException`

This error may occur if you have not started TIBCO EMS before starting the Gateway.

**Solution:** To resolve this error, start TIBCO EMS, then restart the Gateway.

Connection Errors
-----------------

**Cause:** There are two typical scenarios specific to JMS integration with the Gateway:

1.  The Gateway goes offline when a message sent by a JMS client is in transit. The result is that the message is lost and consumers do not get the message even after the Gateway comes back online. (If messages are sent after the Gateway is offline, the KAAZING Gateway JMS client accumulates the messages as pending frames and when the Gateway comes back online, the pending messages are sent.)
2.  The JMS-compliant message broker goes offline and the JMS client sends a message. The result is that messages are queued by the Gateway until the broker comes back online, at which point the messages are sent to the broker and on to subscribers (topics or queues).

**Solution:** From the JMS client side, the KAAZING Gateway Client APIs have several exception classes that can be used to determine what condition resulted in a connection error. For example, see the [Java JMS Client API](http://developer.kaazing.com/documentation/jms/4.0/apidoc/client/java/jms/index.html). A few of the key exception classes are:

-   ConnectionDisconnectedException
-   ConnectionDroppedException
-   ConnectionFailedException
-   ConnectionInterruptedException
-   ConnectionRestoredException
-   ReconnectFailedException

For administrators of the Gateway, examining the error log (`GATEWAY_HOME/log/error.log`) of the Gateway indicates whether the Gateway can connect to the broker. For example, if the broker is offline, a record such as the following will appear in the error log:

`2012-09-20 14:48:32,998 WARN  Unable to establish JMS Connection due to the following exception: Could not connect to broker URL: tcp://localhost:61616. Reason: java.net.ConnectException: Connection refused`

**Notes:**
 
-   When a JMS connection is lost due to the Gateway shutting down or from a WebSocket-level disconnect (for example, the network is suddenly unavailable), the JMS client connection's ExceptionListener `onException()` method is called with a `ConnectionDroppedException` exception.
-   When a JMS connection is lost due to the Gateway connection to the JMS Broker shutting down (for example, the broker is shutdown), then the JMS client's `onException()` method is called with a `ConnectionInterruptedException` exception.
-   When the JMS connection is restored for either of the preceding cases, then the JMS client's `onException()` method is called with a `ConnectionRestoredException` exception.

Exception: A synchronous method call is not permitted
-----------------------------------------------------

**Cause:** When your client attempts to establish more than one distinct topic subscription (meaning either different topics or different message selectors) or if there is more than one queue consumer, you receive the following error message:

`A synchronous method call is not permitted when a session is being used asynchronously: 'createConsumer'.     The JMS specification does not permit the use of a session for synchronous methods when asynchronous message delivery is running.`

Your JMS-compliant message broker strictly enforces the session threading rules of the [JMS 1.1 specification](http://www.oracle.com/technetwork/java/docs-136352.html), section 2.8 Multithreading. Specifically, the Session object does not support concurrent use in JMS 1.1. Session objects may be accessed by one logical thread of control at a time, only. The restriction makes JMS easier to use for some clients.

**Solution:** Add `<workaround>1</workaround>` to the jms service configured on the Gateway. For example:

``` xml
<service>
  <accept>ws://localhost:8001/jms</accept>
  <accept>wss://localhost:9001/jms</accept>
  <type>jms</type>

  <properties>
    <connection.factory.name>ConnectionFactory</connection.factory.name>
    <context.lookup.topic.format>%s</context.lookup.topic.format>
    <context.lookup.queue.format>%s</context.lookup.queue.format>
    <workaround>1</workaround>
    <env.java.naming.factory.initial>
      com.sun.jndi.fscontext.RefFSContextFactory
    </env.java.naming.factory.initial>

    <!-- The location of the .bindings file -->
    <env.java.naming.provider.url>file://tmp</env.java.naming.provider.url>
  </properties>

  <cross-site-constraint>
    <allow-origin>http://localhost:8001</allow-origin>
  </cross-site-constraint>
  <cross-site-constraint>
    <allow-origin>https://localhost:9001</allow-origin>
  </cross-site-constraint>
</service>
```

An Authentication Error Occurs when Trying to Connect the Gateway to the Message Broker
---------------------------------------------------------------------------------------

**Cause:** Authentication errors can occur if the authentication settings on the message broker are changed and the configuration of the Gateway has not been updated.

**Solutions:** The following options are available:

-   Include the [`connection.security.principal`](../admin-reference/r_conf_jms.md#connectionsecurityprincipal) and [`connection.security.credentials`](../admin-reference/r_conf_jms.md#connectionsecuritycredentials) properties in the `jms` service. The Gateway uses these properties for each connection it creates to the message broker. This is the recommended solution. For more information, see [jms](../admin-reference/r_conf_jms.md#jms).
-   Provide the required authentication credentials at each `createConnection()` method call in your KAAZING Gateway JMS client. If you choose this option, the Gateway passes the credentials from the JMS client to the message broker, but the Gateway itself does not authenticate with the message broker. For more information, see `createConnection()` in each of the KAAZING Gateway JMS Client APIs under **Client API Documentation**.
-   Disable authentication at the message broker. This solution should be used to simplify development and testing environments only.

**Note:** When connecting the Gateway to some message brokers, you might need to set the username and password attributes of `ConnectionFactory` in the .bindings file in addition to the above solutions.

Exception: ClassNotFoundException
---------------------------------

**Cause:** A required jar file is missing from the `GATEWAY_HOME/lib` folder.

**Solution:** Review the procedure for integrating KAAZING Gateway with the message broker and ensure that all of the required jar files listed in the procedure are in the `GATEWAY_HOME/lib` folder. To locate the procedure, see [About Integrating KAAZING Gateway and JMS-Compliant Message Brokers](../integration-jms/o_jms_integrate.md).

TIBCO EMS Unable to Free Up Durable Subscribers
-----------------------------------------------

**Cause:** TIBCO EMS is unable to detect connection loss and free up durable subscribers when a Gateway instance shuts down unexpectedly.

**Solution:** To avoid this failure, the following two settings should be put in the TIBCO EMS server configuration file (tibemsd.conf):

-   `client_heartbeat_server` = the value of the Gateway configuration element [`ws.inactivity.timeout`](https://github.com/kaazing/gateway/blob/develop/doc/admin-reference/r_configure_gateway_service.md#wsinactivitytimeout) divided by 3
-   `server_timeout_client_connection` = the value of the Gateway configuration element `ws.inactivity.timeout`

Warning: maximum.pending.acknowledgments has been set to 1
----------------------------------------------------------

**Cause:** The JMS service property [maximum.pending.acknowledgments](../admin-reference/r_conf_jms.md#maximumpendingacknowledgments) has been set to 1 because the JMS provider does not support individual message acknowledgment. Client applications must acknowledge each message received on a durable subscriber or queue in order to receive further messages on that subscriber or queue. Otherwise, the client will receive only one message from that queue or durable subscriber.

**Solution:** If you are using a JMS provider other than Apache ActiveMQ or TIBCO Enterprise Message Service (TIBCO EMS), you must ensure your client applications acknowledge each message received from a queue or durable subscriber.

See Also
--------

For more information about KAAZING Gateway administration, see the [documentation](../index.md).


